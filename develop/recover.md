

## 故障恢复

本章将持续补充...

### yellow状态分析恢复

\`yellow\` 状态表明存在未分配（unassigned）的从分片。

**查询索引状态**

```
curl -s -XGET 'http://<host>:9200/_cat/indices?v'
curl -s -XGET 'http://<host>:9200/_cluster/health?level=indices'
```

**查询未分配的分片**

```
curl -s -XGET 'http://<host>:9200/_cat/shards?v' | grep UNASSIGNED
curl -s -XGET 'http://<host>:9200/_cluster/health?level=shards'
```

\* **索引副本设置不合理**

如果是索引副本数设置大于数据节点个数而导致集群处于 \`yellow\` 状态，修改副本数来修复集群状态

```
curl -XPUT \
http://<host>:9200/unassigned_index/_settings \
-H 'Content-Type: application/json' \
-d '{
    "index": {
        "number_of_replicas": replicasCount
    }
}'

# unassigned_index为未分配的分片索引
# replicasCount为新的索引副本数
```

正常情况下，未分配的从分片会自动得到分配，集群状态也会恢复 \`green\`。某些特殊情况可能需要手动分配掉未分配的从分片

```
curl -XPOST \
http://<host>:9200/_cluster/reroute \
-H 'Content-Type: application/json' \
-d '{
    "commands": [{
        "allocate_replica": {
            "index": "unassigned_index",
            "shard": num,
            "node": "nodeName"
        }
    }]
}'

# unassigned_index为未分配的分片索引
# num为未分配的分片编号
# nodeName为节点名称，也可以为节点编号ID，如kVWViI1PQt2Bk2rP7PlrbQ
```

在放弃和离开分片之前，集群将尝试在一行中分配一个最大的index.allocation.max\\\_retries时间片（默认为5）。
这种情况可能是由结构性问题引起的，例如有一个分析器引用了所有节点上不存在的停用词文件。一旦问题得到解决，可以通过retry\\\_failed调用
\_\[Reroute
API\](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-reroute.html)\_
来手动重试分配，这将试图对这些分片进行一次重试。

```
POST /_cluster/reroute?retry_failed=true
```

集群更糟糕的情况可能出现未分配的主分片，需手动分配请参考
\_**\[Reroute\](<https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-reroute.html>)**\_

\* **节点磁盘使用率超出阀值**

节点磁盘使用率对分片分配的影响，参考 \_**\[Disk-based Shard
Allocation\](<https://www.elastic.co/guide/en/elasticsearch/reference/6.2/disk-allocator.html>)**\_

如果是磁盘使用率高导致集群出现未分配分片，可以选择修改磁盘使用策略暂时缓解 或 扩容节点个数。

另外，如果确定可以永久关闭部分历史索引数据，也可以通过删除此类索引来恢复集群状态。删除历史索引数据可参考 \_**\[实例运维 -
历史数据清理\](<https://docs.ucloud.cn/analysis/ues/develop/online>)**\_
