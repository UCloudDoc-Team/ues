# ES 到 OpenSearch 迁移方案

云搜索服务 CSS（Cloud Search Service）提供 OpenSearch 引擎，完整兼容 Elasticsearch 7.10.2 版本 API，支持存量 ES 业务平滑迁移。本文档介绍将 Elasticsearch 集群数据迁移至 UCloud CSS OpenSearch 集群的标准方案。

## 迁移方案概述

本方案采用「**快照全量迁移 + Logstash 增量同步**」组合策略：

1. 在源 ES 集群创建快照，通过跨云同步工具将快照传输至目标端对象存储
2. 在目标 OpenSearch 集群完成元数据迁移与快照数据恢复
3. 调整索引配置后，通过 Logstash 增量同步追赶快照创建后的增量数据
4. 数据追平后切换业务流量至目标集群

该方案可在源集群不停写的情况下完成全量数据迁移，仅增量追平阶段需短暂停写，将停机窗口压缩至分钟级。

---

## 前置准备

### 源 ES 集群要求

- 账号具备 `cluster:admin/snapshot` 操作权限
- 待迁移索引存在可用的时间戳字段（供 Logstash 增量过滤使用）
- 网络可达对象存储（用于快照导出）及目标端 Logstash（用于增量同步）

### 目标 OpenSearch 集群要求

- 已创建 OpenSearch 集群，集群 HTTPS 可访问，管理员账号可用
- 存储容量满足源集群数据量需求（建议预留 15% 以上余量）
- 目标集群版本 ≥ 源 ES 版本（OpenSearch 3.x 兼容 ES 7.10.2 及之后版本的快照格式）

### 对象存储准备

需准备两个对象存储 Bucket：

| Bucket | 位置 | 用途 |
|---|---|---|
| 源端 Bucket | 与源 ES 集群同地域 | 存放源集群快照 |
| 目标端 Bucket | 与目标 OpenSearch 同地域 | 存放同步后的快照，供目标集群恢复 |

两个 Bucket 均需准备 Access Key / Secret Key，具备读写权限。

### 执行机准备

在目标集群所在地域创建一台云主机作为迁移执行机，要求：

- 内网可访问目标端对象存储与目标 OpenSearch 集群
- 公网可访问源端对象存储（用于快照跨云拉取）
- 安装以下工具：
  - [us3sync](https://docs.ucloud.cn/ufile/tools/us3sync/introduction)：快照跨云同步工具
  - kubectl：K8s 集群操作客户端

---

## 迁移步骤

### 步骤一：注册快照仓库

在可访问源 ES 集群的机器上执行（无需停写），将源端对象存储注册为快照仓库：

```bash
curl -X PUT "http://<es-host>:9200/_snapshot/migration-repo" \
  -H 'Content-Type: application/json' \
  -u <username>:<password> \
  -d '{
    "type": "s3",
    "settings": {
      "bucket": "<bucket-name>",
      "base_path": "snapshot/migration",
      "endpoint": "<s3-endpoint>",
      "region": "<region>",
      "access_key": "<access_key>",
      "secret_key": "<secret_key>"
    }
  }'
```

| 参数 | 说明 |
|---|---|
| `type` | 对象存储类型，S3 兼容接口填 `s3`，阿里云 OSS 填 `oss` |
| `bucket` | 源端 Bucket 名称 |
| `endpoint` | 对象存储 Service Endpoint |
| `base_path` | 快照在 Bucket 中的存储路径前缀 |

---

### 步骤二：创建快照

```bash
# 异步创建快照
curl -X PUT "http://<es-host>:9200/_snapshot/migration-repo/<snapshot-name>?wait_for_completion=false" \
  -H 'Content-Type: application/json' \
  -u <username>:<password> \
  -d '{"include_global_state": false}'
```

查看快照进度：

```bash
curl "http://<es-host>:9200/_snapshot/migration-repo/<snapshot-name>/_status?pretty" \
  -u <username>:<password>
```

等待 `state` 字段为 `SUCCESS`。

---

### 步骤三：快照跨云同步

使用 us3sync 工具将快照数据从源端 Bucket 同步至目标端 Bucket：

```bash
# 安装 us3sync
# 参考 https://docs.ucloud.cn/ufile/solutions

chmod +x us3sync

# 执行跨云同步
./us3sync sync \
  --source_endpoint <source-endpoint> \
  --source_ak <source-access-key> \
  --source_sk <source-secret-key> \
  --source_bucket <source-bucket> \
  --target_endpoint <target-endpoint> \
  --target_ak <target-access-key> \
  --target_sk <target-secret-key> \
  --target_bucket <target-bucket>
```

> 跨云同步耗时取决于快照数据量与跨云带宽。建议提前评估数据量，配置充足带宽。

---

### 步骤四：元数据迁移

将快照中的索引元数据（mapping、settings 等）迁移至目标 OpenSearch 集群，在目标集群创建对应的空索引。

**配置迁移参数：**

| 变量 | 说明 |
|---|---|
| `S3_REPO_URI` | 目标端 Bucket 中快照的路径 |
| `SNAPSHOT_NAME` | 快照名称 |
| `S3_REGION` / `S3_ENDPOINT` | 目标端对象存储的 region 与内网 endpoint |
| `TARGET_HOST` | 目标 OpenSearch 集群地址 |
| `ALLOW_EXISTING_INDEXES` | 目标侧索引已存在时是否继续（默认 `true`） |
| `INDEX_ALLOWLIST` | 仅迁移指定索引（逗号分隔）；留空则迁移全部 |

执行元数据迁移：

```bash
cd deploy/metadata/
cp migration.env.example migration.env
$EDITOR migration.env   # 填入快照路径、目标集群地址与凭据
bash run.sh
```

查看执行日志与结果：

```bash
# 查看迁移任务日志
kubectl -n <namespace> logs -f -l app.kubernetes.io/name=metadata-migration

# 确认 Job 完成
kubectl -n <namespace> get job metadata-migration
```

---

### 步骤五：调整索引 Settings 与分片数

元数据迁移完成后、数据恢复之前，需根据业务需求调整目标索引的配置。

**对比源与目标索引的 settings / mapping：**

```bash
# 导出源集群配置
curl -s "http://<es-host>:9200/<index-name>/_settings?pretty" \
  -u <username>:<password> > /tmp/src-settings.json
curl -s "http://<es-host>:9200/<index-name>/_mapping?pretty" \
  -u <username>:<password> > /tmp/src-mapping.json

# 导出目标集群配置
curl -s -k "https://<opensearch-host>:9200/<index-name>/_settings?pretty" \
  -u <username>:<password> > /tmp/tgt-settings.json
curl -s -k "https://<opensearch-host>:9200/<index-name>/_mapping?pretty" \
  -u <username>:<password> > /tmp/tgt-mapping.json

# 对比差异
diff -u /tmp/src-settings.json /tmp/tgt-settings.json
diff -u /tmp/src-mapping.json /tmp/tgt-mapping.json
```

> settings diff 中 `uuid`、`creation_date`、`provided_name` 等集群内部元数据的差异可忽略；重点关注 `number_of_shards`、`number_of_replicas`、`refresh_interval`、`index.sort` 及 mapping 字段类型。

**调整策略：**

- **可变 settings**（副本数、刷新间隔等），直接 PUT 修改：

```bash
curl -k -X PUT "https://<opensearch-host>:9200/<index-name>/_settings" \
  -H 'Content-Type: application/json' -u <username>:<password> \
  -d '{"index": {"number_of_replicas": 0, "refresh_interval": "30s"}}'
```

- **不可变 settings**（分片数），需导出 mapping → 删除索引 → 按新分片数重建：

```bash
# 导出 mapping
curl -k "https://<opensearch-host>:9200/<index-name>/_mapping?pretty" \
  -u <username>:<password> > /tmp/mapping.json

# 删除旧索引
curl -k -X DELETE "https://<opensearch-host>:9200/<index-name>" -u <username>:<password>

# 按目标分片数重建
curl -k -X PUT "https://<opensearch-host>:9200/<index-name>" \
  -H 'Content-Type: application/json' -u <username>:<password> \
  -d '{
    "settings": {
      "number_of_shards": <target-shards>,
      "number_of_replicas": 0,
      "index": {"sort": {"<field>": "asc"}}
    },
    "mappings": <mapping-from-export>
  }'
```

> **注意**：调整分片数需在数据恢复之前完成。

---

### 步骤六：快照数据恢复（Reindex from Snapshot）

此步骤将快照中的实际数据并行写入目标集群。源集群无需停写。

多个 Worker 通过目标集群上的协调索引（`.migrations_working_state`）自动分配分片，互不重复。

**Worker 数量建议**：初始 8~16 个，根据目标集群写入负载动态调整。

#### 清理协调状态（仅重跑时执行）

如果是首次执行，跳过此步。如需重跑：

```bash
curl -k -X DELETE "https://<opensearch-host>:9200/.migrations_working_state*" \
  -u <username>:<password>
```

#### 配置参数

| 变量 | 说明 |
|---|---|
| `NAMESPACE` | K8s namespace |
| `RFS_WORKERS` | 并行 Worker 数量 |
| `RFS_WORK_STORAGE` | 每个 Worker 的 PVC 大小（建议为最大单分片大小的 2 倍） |
| `RFS_S3_REPO_URI` / `RFS_SNAPSHOT_NAME` | 目标端 Bucket 中快照的路径与名称 |
| `RFS_SESSION_NAME` | 协调索引后缀；重跑时修改以隔离上次状态 |

#### 部署与启动

```bash
# 创建 namespace（如不存在）
kubectl create namespace <namespace>

# 启动 RFS Worker
bash run.sh
```

#### 动态扩缩容

```bash
bash scripts/deploy.sh scale 16   # 调整至 16 个 Worker
```

#### 监控进度

```bash
# 查看落地速率（docs/s、MB/s、进度百分比、预估剩余时间）
bash scripts/monitor.sh rate

# 查看 Worker 日志
bash scripts/monitor.sh logs
```

#### 结束判定

同时满足以下条件即表示数据恢复完成：

1. Worker 日志出现 `NO_WORK_LEFT` 或 `NO_WORK_AVAILABLE`
2. K8s Job 状态变为 `Complete`
3. 目标集群文档数与快照中文档数一致

#### 清理资源

恢复完成后清理 Job 与 PVC（Secret 保留）：

```bash
bash scripts/deploy.sh cleanup
```

---

### 步骤七：恢复索引配置

数据恢复完成后，将索引副本数等配置恢复为生产状态：

```bash
curl -k -X PUT "https://<opensearch-host>:9200/<index-name>/_settings" \
  -H 'Content-Type: application/json' -u <username>:<password> \
  -d '{"index": {"number_of_replicas": 1, "refresh_interval": "30s"}}'

curl -k -X POST "https://<opensearch-host>:9200/<index-name>/_refresh" \
  -u <username>:<password>
```

等待 `_cat/shards` 中所有副本分片状态为 `STARTED`。

---

### 步骤八：Logstash 增量同步

快照创建后源集群持续产生的增量数据，通过 Logstash 追赶同步至目标集群。

#### 查询增量起点

每轮同步前，先查询目标集群中数据的最大时间戳：

```bash
curl -s -k "https://<opensearch-host>:9200/<index-name>/_search" \
  -u <username>:<password> \
  -H 'Content-Type: application/json' \
  -d '{"size":0,"aggs":{"max_ts":{"max":{"field":"<timestamp-field>"}}}}'
```

取聚合结果中的 `max_ts` 值作为本轮 `SYNC_SINCE`。

> 默认时间戳格式为 `epoch_second`（秒级整数）。若字段为 ISO 格式（如 `2024-01-01T00:00:00Z`），需将 `LS_TIMESTAMP_FORMAT` 设为 `strict_date_optional_time`，取 `value_as_string` 作为 `SYNC_SINCE`。

#### 运行 Logstash

```bash
# 单实例（默认）
SYNC_SINCE=<timestamp> bash logstash/run-logstash.sh

# 多实例并发（每实例处理 1/N 源分片，LS_INSTANCES 上限 = 源分片数）
LS_INSTANCES=4 SYNC_SINCE=<timestamp> bash logstash/run-logstash.sh
```

#### 监控

```bash
# 查看所有实例日志
kubectl -n <namespace> logs -f -l app.kubernetes.io/name=logstash-incremental

# 查看 Job 状态
kubectl -n <namespace> get job -l app.kubernetes.io/name=logstash-incremental
```

所有 Job `COMPLETIONS` 变为 `1/1` 即本轮完成。

#### 运行策略

| 轮次 | SYNC_SINCE 取值 | 说明 |
|---|---|---|
| 第一轮 | 目标集群 `max(timestamp)` | 追赶快照创建至当前时刻的增量 |
| 后续轮次 | 上一轮的 `EXEC_TIMESTAMP` | 每轮缩短追赶窗口 |
| 最后一轮（停写后） | 上一轮的 `EXEC_TIMESTAMP` | 停写源集群后追平最后一批增量 |

> `gte` 为闭区间查询，边界重叠的数据依靠 `document_id` 幂等覆盖，不会产生重复。Job 异常中断时不更新 `SYNC_SINCE`，直接原值重跑。

#### 数据校验

```bash
# 对比源与目标索引文档数
curl -s "http://<es-host>:9200/<index-name>/_count" -u <username>:<password>
curl -k -s "https://<opensearch-host>:9200/<index-name>/_count" -u <username>:<password>
```

---

### 步骤九：流量切换

确认增量数据追平且校验通过后，将业务读写切换至目标 OpenSearch 集群。

1. 停写源 ES 集群
2. 执行最后一轮 Logstash 增量同步，追平剩余增量
3. 最终校验文档数与 mapping 一致
4. 修改业务配置，将读写端点指向目标 OpenSearch 集群地址
