# 插件管理

## Elasticsearch-Head

最新elasticsearch版本（5.0以上）已经不再支持使用内部 **elasticsearch-plugin**
命令来安装head插件，内置的head插件本质是调用es服务REST
API包装而形成的图形化管理工具，所以使用head插件需要head管理平台网络和输入的es连接地址是相通的。

## Hadoop HDFS Repository Plugin

\* 通过REST API定义hdfs存储库的配置：

```
PUT /_snapshot/my_hdfs_repository
{
  "type": "hdfs",
  "settings": {
    "uri": "hdfs://namenode:8020/",
    "path": "elasticsearch/respositories/my_hdfs_repository"
  }
}

# uri: hdfs的地址，例如："hdfs://<host>:<port>/"
# path: 数据存储/加载的文件系统中的文件路径，例如："path/to/file"
```

创建成功，可以获取hdfs仓库信息：

```
GET /_snapshot/my_hdfs_repository
```

示例

    {
      "my_hdfs_repository": {
        "type": "hdfs",
        "settings": {
          "path": "elasticsearch/respositories/my_hdfs_repository",
          "uri": "hdfs://namenode:8020/"
        }
      }
    }

\* 创建快照：

```
PUT /_snapshot/my_hdfs_repository/snapshot_1
{
  "indices": "index_1,index_2",
  "ignore_unavailable": true,
  "include_global_state": false
}
```

创建成功，可以获取快照信息：

```
GET /_snapshot/my_hdfs_repository/snapshot_1
```

示例

    {
      "snapshots": [
        {
          "snapshot": "snapshot_1",
          "uuid": "yr9T6jtLTCeVFRoNGN-9Lw",
          "version_id": 5050199,
          "version": "5.5.1",
          "indices": [
            ".kibana",
            "ucloud"
          ],
          "state": "SUCCESS",
          "start_time": "2018-02-01T08:13:26.128Z",
          "start_time_in_millis": 1517472806128,
          "end_time": "2018-02-01T08:13:28.870Z",
          "end_time_in_millis": 1517472808870,
          "duration_in_millis": 2742,
          "failures": [],
          "shards": {
            "total": 6,
            "failed": 0,
            "successful": 6
          }
        }
      ]
    }

\* 删除快照：

```
DELETE /_snapshot/my_hdfs_repository/snapshot_1
```

\* 快照恢复：

```
POST /_snapshot/my_hdfs_repository/snapshot_1/_restore
```

**注意：** 若集群中已经存在需要快照恢复的索引并且为 open 状态，需要使用 \`\_close\` API先关闭该索引，示例：

```
POST /.kibana/_close
```

更详细插件使用请参考 \_\[Hadoop HDFS Repository
Plugin\](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html\#\_repository\_plugins)\_

## IK Analysis Plugin

### 自定义分词词库操作

通过在 IK 配置文件中提到的如下配置：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <comment>IK Analyzer 扩展配置</comment>
    <!--用户可以在这里配置自己的扩展字典 -->
    <entry key="ext_dict">custom/mydict.dic;custom/single_word_low_freq.dic</entry>
    <!--用户可以在这里配置自己的扩展停止词字典-->
    <entry key="ext_stopwords">custom/ext_stopword.dic</entry>
    <!--用户可以在这里配置远程扩展字典 -->
    <entry key="remote_ext_dict">location</entry>
    <!--用户可以在这里配置远程扩展停止词字典-->
    <entry key="remote_ext_stopwords">http://xxx.com/xxx.dic</entry>
</properties>
```

IK分词支持本地自定义词库，并且支持远程热更新词库，UES通过 \*\* 词库信息保存在特定索引中 \*\* 来完成自定义词库的更新。下面给出在
\*\* Kibana \*\* 中的具体操作流程，其中 \*\* API 要保持完全一致 \*\*。

\* \*\* 本地扩展字典 \*\*

```
PUT /custom_ik/analyzer/1
{
  "ext_dict": [
  ]
}
```

示例：

```
PUT /custom_ik/analyzer/1
{
  "ext_dict": [
    "中华人民共和国",
    "美利坚合众国",
    "大不列颠和北爱尔兰联合王国"
  ]
}
```

\* \*\* 本地扩展停止词字典 \*\*

```
PUT /custom_ik/analyzer/2
{
  "ext_stopwords": [
  ]
}
```

示例：

```
PUT /custom_ik/analyzer/2
{
  "ext_stopwords": [
    "中华人民共和国",
    "美利坚合众国",
    "大不列颠和北爱尔兰联合王国"
  ]
}
```

\* \*\* 远程扩展字典 \*\*

```
PUT /custom_ik/analyzer/3
{
  "remote_ext_dict": ""
}
```

示例：

```
PUT /custom_ik/analyzer/3
{
  "remote_ext_dict": "http://localhost:8080/my_dict.dic"
}
```

\* \*\* 远程扩展停止词字典 \*\*

```
PUT /custom_ik/analyzer/4
{
  "remote_ext_stopwords": ""
}
```

示例：

```
PUT /custom_ik/analyzer/4
{
  "remote_ext_stopwords": "http://localhost:8080/my_stopwords.dic"
}
```

检测索引数据是否成功

```
GET /custom_ik/analyzer/1
GET /custom_ik/analyzer/2
GET /custom_ik/analyzer/3
GET /custom_ik/analyzer/4
```

\*\* 最后还需一步 \*\*，到控制台点击需要更新的分词词库项完成配置文件的更新。