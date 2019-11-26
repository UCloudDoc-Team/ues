

## 实例运维

生产环境的集群要健康运行及故障处理需要合理、高效的运营经验。本章将提供比较实用的在线运营实例。

### 集群健康

在 Elasticsearch 集群中可以监控统计很多信息，其中最重要的就是：**集群健康(Cluster Health)**。它的
status 有 \`Green\`、\`Yellow\`、\`Red\` 三种。

```
GET /_cluster/health
```

系统返回：

    {
      "cluster_name" : "ues-qwerty",
      "status" : "green",
      "timed_out" : false,
      "number_of_nodes" : 3,
      "number_of_data_nodes" : 3,
      "active_primary_shards" : 1,
      "active_shards" : 2,
      "relocating_shards" : 0,
      "initializing_shards" : 0,
      "unassigned_shards" : 0,
      "delayed_unassigned_shards" : 0,
      "number_of_pending_tasks" : 0,
      "number_of_in_flight_fetch" : 0,
      "task_max_waiting_in_queue_millis" : 0,
      "active_shards_percent_as_number" : 100.0
    }

响应信息中的一块就是 \`status\` ，这是我们最应该关注的字段，它告诉我们当前集群是否处于一个可用的状态。其中三种颜色分别代表：

|            |                                           |                                                                                                                |
| ---------- | ----------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| 状态         | 描述                                        | 备注                                                                                                             |
| \`green\`  | 所有主分片和从分片都可用                              | 所有的主分片和副本分片都已分配。集群是 100% 可用的。                                                                                  |
| \`yellow\` | 所有主分片可用，但存在不可用的从分片，即存在未分配（unassigned）的从分片 | 所有的主分片已经分片了，但至少还有一个副本是缺失的。不会有数据丢失，所以搜索结果依然是完整的。不过，你的高可用性在某种程度上被弱化。如果更多的分片消失，会丢数据了。把 \`yellow\` 想象成一个需要及时调查的警告。 |
| \`red\`    | 存在不可用的主要分片，即存在未分配（unassigned）的主分片         | 至少一个主分片（以及它的全部副本）都在缺失中。这意味着你在缺少数据：搜索只能返回部分数据，而分配到这个分片上的写入请求会返回一个异常。                                            |

为保障集群为 \`green\` 状态，即所有主从分片都可用。强烈建议**索引主分片设置小于数据节点个数的3倍，副本设置小于数据节点个数**。

### 单个节点监控

集群健康是对集群的所有信息进行高度概述，而节点统计值 API 提供一个让人眼花缭乱的统计数据的数组，包含集群的每一个节点统计值。

节点统计值 API 可以通过如下命令执行：

```
GET /_nodes/stats
```

在输出内容的开头，我们可以看到集群名称和我们的第一个节点：

    {
        "_nodes": {
            "total": 3,
            "successful": 3,
            "failed": 0
        },
        "cluster_name": "ues-qwerty",
        "nodes": {
            "cZabQfdFTVCOdh9Zxrjocg": {
                "timestamp": 1515997798238,
                "name": "ues-qwerty-01",
                "transport_address": "192.168.1.1:9300",
                "host": "192.168.1.1",
                "ip": "192.168.1.1:9300",
                "roles": [
                    "master",
                    "data",
                    "ingest"
                ],
                "indices": {
    ...

节点是排列在一个哈希里，以节点的 UUID
作为键名。还显示了节点网络属性的一些信息，这些值对调试诸如节点未加入集群这类自动发现问题很有用。通常你会发现是端口用错了，或者节点绑定在错误的
IP 地址/网络接口上了。

#### 索引部分

索引（indices）部分列出了这个节点上所有索引的聚合过的统计值 ：

``` 
    "indices": {
        "docs": {
           "count": 0,
           "deleted": 0
        },
        "store": {
           "size_in_bytes": 0,
           "throttle_time_in_millis": 0
        },
```

\* docs 展示节点内存有多少文档，包括还没有从段里清除的已删除文档数量。

\* store 部分显示节点耗用了多少物理存储。这个指标包括主分片和副本分片在内。如果限流时间很大，那可能表明你的磁盘限流设置得过低。

``` 
        "indexing": {
                    "index_total": 2,
                    "index_time_in_millis": 146,
                    "index_current": 0,
                    "index_failed": 0,
                    "delete_total": 1,
                    "delete_time_in_millis": 6,
                    "delete_current": 0,
                    "noop_update_total": 0,
                    "is_throttled": false,
                    "throttle_time_in_millis": 0
                },
                "get": {
                    "total": 2,
                    "time_in_millis": 7,
                    "exists_total": 2,
                    "exists_time_in_millis": 7,
                    "missing_total": 0,
                    "missing_time_in_millis": 0,
                    "current": 0
                },
                "search": {
                    "open_contexts": 0,
                    "query_total": 50,
                    "query_time_in_millis": 74,
                    "query_current": 0,
                    "fetch_total": 40,
                    "fetch_time_in_millis": 25,
                    "fetch_current": 0,
                    "scroll_total": 0,
                    "scroll_time_in_millis": 0,
                    "scroll_current": 0,
                    "suggest_total": 0,
                    "suggest_time_in_millis": 0,
                    "suggest_current": 0
                },
                "merges": {
                    "current": 0,
                    "current_docs": 0,
                    "current_size_in_bytes": 0,
                    "total": 0,
                    "total_time_in_millis": 0,
                    "total_docs": 0,
                    "total_size_in_bytes": 0,
                    "total_stopped_time_in_millis": 0,
                    "total_throttled_time_in_millis": 0,
                    "total_auto_throttle_in_bytes": 104857600
                },
```

\* indexing
显示已经索引了多少文档。这个值是一个累加计数器，在文档被删除时，数值不会下降，在发生文档更新等内部索引操作时，值会增加。此外还列出了索引操作耗费的时间，正在索引的文档数量，以及删除操作的类似统计值。

\* get 显示通过 ID 获取文档的接口相关的统计值。包括对单个文档的 GET 和 HEAD 请求。

\* search 描述在活跃中的搜索（open\_contexts）数量、查询的总数量、以及自节点启动以来在查询上消耗的总时间。用
query\\\_time\\\_in\\\_millis / query\\\_total
计算的比值，可以用来粗略的评价你的查询有多高效。比值越大，每个查询花费的时间越多，你应该要考虑调优了。

    fetch 统计值展示了查询处理的后一半流程（query-then-fetch 里的 fetch）。如果 fetch 耗时比 query 还多，说明磁盘较慢，或者获取了太多文档，或者可能搜索请求设置了太大的分页（比如，size: 10000）。

\* merges 包括了 Lucene
段合并相关的信息。它会告诉你目前在运行几个合并，合并涉及的文档数量，正在合并的段的总大小，以及在合并操作上消耗的总时间。

    在你的集群写入压力很大时，合并统计值非常重要。合并要消耗大量的磁盘 I/O 和 CPU 资源。如果你的索引有大量的写入，同时又发现大量的合并数，一定要去阅读_**[索引性能技巧](https://www.elastic.co/guide/cn/elasticsearch/guide/current/indexing-performance.html)**_。

``` 
        "filter_cache": {
           "memory_size_in_bytes": 48,
           "evictions": 0
        },
        "fielddata": {
           "memory_size_in_bytes": 0,
           "evictions": 0
        },
        "segments": {
           "count": 319,
           "memory_in_bytes": 65812120
        },
        ...
```

\* filter\\\_cache 展示了已缓存的过滤器位集合所用的内存数量，以及过滤器被驱逐出内存的次数。过多的驱逐数 可能
说明你需要加大过滤器缓存的大小，或者你的过滤器不太适合缓存（比如它们因为高基数而在大量产生，就像是缓存一个
now 时间表达式）。

    不过，驱逐数是一个很难评定的指标。过滤器是在每个段的基础上缓存的，而从一个小的段里驱逐过滤器，代价比从一个大的段里要廉价的多。有可能你有很大的驱逐数，但是它们都发生在小段上，也就意味着这些对查询性能只有很小的影响。把驱逐数指标作为一个粗略的参考。如果你看到数字很大，检查一下你的过滤器，确保他们都是正常缓存的。不断驱逐着的过滤器，哪怕都发生在很小的段上，效果也比正确缓存住了的过滤器差很多。

\* field\\\_data 显示 fielddata 使用的内存， 用以聚合、排序等等。这里也有一个驱逐计数。和
filter\\\_cache 不同的是，这里的驱逐计数是很有用的：这个数应该或者至少是接近于 0。因为 fielddata
不是缓存，任何驱逐都消耗巨大，应该避免掉。如果你在这里看到驱逐数，你需要重新评估你的内存情况，fielddata
限制，请求语句，或者这三者。

\* segments 会展示这个节点目前正在服务中的 Lucene 段的数量。 这是一个重要的数字。大多数索引会有大概 50–150
个段，哪怕它们存有 TB
级别的数十亿条文档。段数量过大表明合并出现了问题（比如，合并速度跟不上段的创建）。注意这个统计值是节点上所有索引的汇聚总数。

    memory 统计值展示了 Lucene 段自己用掉的内存大小。 这里包括底层数据结构，比如倒排表，字典，和布隆过滤器等。太大的段数量会增加这些数据结构带来的开销，这个内存使用量就是一个方便用来衡量开销的度量值。

#### 操作系统和进程部分

\`OS\` 和 \`Process\` 部分基本是自描述的，不会在细节中展开讲解。它们列出来基础的资源统计值，比如 CPU 和负载。OS
部分描述了整个操作系统，而 Process 部分只显示 Elasticsearch 的 JVM 进程使用的资源情况。

这些都是非常有用的指标，不过通常在你的监控技术栈里已经都测量好了。统计值包括下面这些：

\* CPU

\* 负载

\* 内存使用率

\* Swap 使用率

\* 打开的文件描述符

#### JVM部分

\`jvm\` 部分包括了运行 Elasticsearch 的 JVM 进程一些很关键的信息。最重要的，它包括了垃圾回收的细节，这对你的
Elasticsearch 集群的稳定性有着重大影响。

``` 
            "jvm": {
                "timestamp": 1515997798245,
                "uptime_in_millis": 1704518268,
                "mem": {
                    "heap_used_in_bytes": 143523960,
                    "heap_used_percent": 3,
                    "heap_committed_in_bytes": 4277534720,
                    "heap_max_in_bytes": 4277534720,
                    "non_heap_used_in_bytes": 95476824,
                    "non_heap_committed_in_bytes": 102416384,
```

#### 线程池部分

Elasticsearch
在内部维护了线程池。这些线程池相互协作完成任务，有必要的话相互间还会传递任务。通常来说，你不需要配置或者调优线程池，不过查看它们的统计值有时候还是有用的，可以洞察你的集群表现如何。

这有一系列的线程池，但以相同的格式输出：

``` 
  "index": {
     "threads": 1,
     "queue": 0,
     "active": 0,
     "rejected": 0,
     "largest": 1,
     "completed": 1
  }
```

每个线程池会列出已配置的线程数量（threads），当前在处理任务的线程数量（active），以及在队列中等待处理的任务单元数量（queue）。

如果队列中任务单元数达到了极限，新的任务单元会开始被拒绝，你会在 rejected
统计值上看到它反映出来。这通常是你的集群在某些资源上碰到瓶颈的信号。因为队列满意味着你的节点或集群在用较高的速度运行，但依然跟不上工作的蜂拥而入。

值得关注的线程池部分有：

\* indexing

普通的索引请求的线程池

\* bulk

批量请求，和单条的索引请求不同的线程池

\* get

Get-by-ID 操作

\* search

所有的搜索和查询请求

\* merging

专用于管理 Lucene 合并的线程池

#### 文件系统和网络部分

继续向下阅读 /\_node/stats API，你会看到一串和你的文件系统相关的统计值：可用空间，数据目录路径，磁盘 I/O
统计值，等等。如果你没有监控磁盘可用空间的话，可以从这里获取这些统计值。磁盘 I/O
统计值也很方便，不过通常那些更专门的命令行工具（比如 iostat ）会更有用些。

显然，Elasticsearch 在磁盘空间满的时候很难运行——所以请确保不会这样。

还有两个跟 网络统计值相关的部分：

``` 
            "transport": {
                "server_open": 26,
                "rx_count": 8834703,
                "rx_size_in_bytes": 19706396482,
                "tx_count": 8834702,
                "tx_size_in_bytes": 18212792172
            },
            "http": {
                "current_open": 3,
                "total_opened": 153
            },
```

\* transport 显示和 传输地址 相关的一些基础统计值。包括节点间的通信（通常是 9300
端口）以及任意传输客户端或者节点客户端的连接。如果看到这里有很多连接数不要担心；Elasticsearch
在节点之间维护了大量的连接。

\* http 显示 HTTP 端口（通常是 9200）的统计值。如果你看到 total\_opened
数很大而且还在一直上涨，这是一个明确信号，说明你的 HTTP
客户端里有没启用 keep-alive 长连接的。持续的 keep-alive
长连接对性能很重要，因为连接、断开套接字是很昂贵的（而且浪费文件描述符）。请确认你的客户端都配置正确。

#### 断路有

fielddata 断路器相关的统计值：

``` 
                "fielddata": {
                    "limit_size_in_bytes": 2566520832,
                    "limit_size": "2.3gb",
                    "estimated_size_in_bytes": 0,
                    "estimated_size": "0b",
                    "overhead": 1.03,
                    "tripped": 0
                },
```

这里你可以看到断路器的最大值（比如，一个请求申请更多的内存时会触发断路器）。这个部分还会让你知道断路器被触发了多少次，以及当前配置的间接开销。间接开销用来铺垫评估，因为有些请求比其他请求更难评估。

主要需要关注的是 tripped
指标。如果这个数字很大或者持续上涨，这是一个信号，说明你的请求需要优化，或者你需要添加更多内存（单机上添加，或者通过添加新节点的方式）。

### 集群统计

集群统计 API 提供了和 节点统计 相似的输出。 但有一个重要的区别：节点统计显示的是每个节点上的统计值，而 集群统计
展示的是对于单个指标，所有节点的总和值。

这里面提供一些很值得一看的统计值。比如说你可以看到，整个集群用了 50%
的堆内存，或者说过滤器缓存的驱逐情况不严重。这个接口主要用途是提供一个比
集群健康 更详细、但又没有 节点统计 那么详细的快速概览。对于非常大的集群来说也很有用，因为那时候 节点统计 的输出已经非常难于阅读了。

这个 API 可以像下面这样调用：

```
GET /_cluster/stats
```

### 索引统计

有时候我们想从 索引为中心 的角度看统计值：索引 收到了多少个搜索请求？索引 获取文档耗费了多少时间？

要做到这点，选择你感兴趣的索引 （ 或者多个索引 ）然后执行一个索引级别的 统计 API：

```
# 统计 my_index 索引
GET /my_index/_stats 

# 使用逗号分隔索引名可以请求多个索引统计值
GET /my_index,another_index/_stats 

# 使用特定的 _all 可以请求全部索引的统计值
GET /_all/_stats 
```

返回的统计信息和 节点统计 的输出很相似：search 、 fetch 、 get 、 index 、 bulk 、 segment
counts 等等。

索引为中心的统计在有些时候很有用，比如辨别或验证集群中的 热门 索引，或者试图找出某些索引比其他索引更快或者更慢的原因。

实践中，节点为中心的统计还是显得更有用些。瓶颈往往是针对整个节点而言，而不是对于单个索引。因为索引一般是分布在多个节点上的，这导致以索引为中心的统计值通常不是很有用，因为它们是从不同环境的物理机器上汇聚的数据。

索引为中心的统计作为一个有用的工具可以保留在你的技能表里，但是通常它不会是第一个用的上的工具。

### 等待中的任务

有一些任务只能由主节点去处理，比如创建一个新的索引或者在集群中移动分片。由于一个集群中只能有一个主节点，所以只有这一节点可以处理集群级别的元数据变动。在
99.9999% 的时间里，这不会有什么问题。元数据变动的队列基本上保持为零。

在一些 罕见 的集群里，元数据变动的次数比主节点能处理的还快。这会导致等待中的操作会累积成队列。

等待中的任务 API 会给你展示队列中（如果有的话）等待的集群级别的元数据变更操作：

```
GET /_cluster/pending_tasks
```

通常，响应都是像这样的：

    {
       "tasks": []
    }

### cat API

cat API 对一些集群信息获取会非常有用。

通过 GET 请求发送 cat 命名可以列出所有可用的 API：

```
GET /_cat
```

    =^.^=
    /_cat/allocation
    /_cat/shards
    /_cat/shards/{index}
    /_cat/master
    /_cat/nodes
    /_cat/tasks
    /_cat/indices
    /_cat/indices/{index}
    /_cat/segments
    /_cat/segments/{index}
    /_cat/count
    /_cat/count/{index}
    /_cat/recovery
    /_cat/recovery/{index}
    /_cat/health
    /_cat/pending_tasks
    /_cat/aliases
    /_cat/aliases/{alias}
    /_cat/thread_pool
    /_cat/thread_pool/{thread_pools}
    /_cat/plugins
    /_cat/fielddata
    /_cat/fielddata/{fields}
    /_cat/nodeattrs
    /_cat/repositories
    /_cat/snapshots/{repository}
    /_cat/templates

### 动态变更设置

Elasticsearch 里很多设置都是动态的，可以通过 API 修改。需要强制重启节点（或者集群）的配置修改都要极力避免。
而且虽然通过静态配置项也可以完成这些变更，我们建议你还是用 API 来实现。

集群更新 API 有两种工作模式：

\* 临时（Transient）

这些变更在集群重启之前一直会生效。一旦整个集群重启，这些配置就被清除。

\* 永久（Persistent）

这些变更会永久存在直到被显式修改。即使全集群重启它们也会存活下来并覆盖掉静态配置文件里的选项。

临时或永久配置需要在 JSON 体里分别指定：

```
PUT /_cluster/settings
{
    "persistent" : {
        "discovery.zen.minimum_master_nodes" : 2 
    },
    "transient" : {
        "indices.store.throttle.max_bytes_per_sec" : "50mb" 
    }
}

# 这个永久设置会在全集群重启时存活下来
# 这个临时设置会在第一次全集群重启后被移除
```

### 日志记录

Elasticsearch 会输出很多日志，默认的日志记录等级是 INFO 。它提供了适度的信息，但是又设计好了不至于让你的日志太过庞大。

当调试问题的时候，特别是节点发现相关的问题（因为这个经常依赖于各式过于繁琐的网络配置），提高日志记录等级到 DEBUG 是很有帮助的。

调高节点发现的日志记录级别：

```
PUT /_cluster/settings
{
    "transient" : {
        "logger.discovery" : "DEBUG"
    }
}
```

#### 慢日志

还有另一个日志叫 慢日志 。这个日志的目的是捕获那些超过指定时间阈值的查询和索引请求。这个日志用来追踪由用户产生的很慢的请求很有用。

默认情况，慢日志是不开启的。要开启它，需要定义具体动作（query，fetch 还是 index），你期望的事件记录等级（ WARN 、
DEBUG 等），以及时间阈值。

这是一个索引级别的设置，也就是说可以独立应用给单个索引：

```
PUT /my_index/_settings
{
    "index.search.slowlog.threshold.query.warn" : "10s", 
    "index.search.slowlog.threshold.fetch.debug": "500ms", 
    "index.indexing.slowlog.threshold.index.info": "5s" 
}

# 查询慢于 10 秒输出一个 WARN 日志
# 获取慢于 500 毫秒输出一个 DEBUG 日志
# 索引慢于 5 秒输出一个 INFO 日志
```

一旦阈值设置过了，你可以和其他日志器一样切换日志记录等级：

```
PUT /_cluster/settings
{
    "transient" : {
        "logger.index.search.slowlog" : "DEBUG", 
        "logger.index.indexing.slowlog" : "WARN" 
    }
}

# 设置搜索慢日志为 DEBUG 级别
# 设置索引慢日志为 WARN 级别
```

### 历史数据清理

随着数据的不断写入，节点的磁盘使用率就会不断攀升，超过一定阀值就会出现不能分配分片的现象。一种解决方法就是删除部分索引数据来使集群节点的磁盘使用率保持在一种合理状态。

删除历史数据，可以通过网络互通的 云主机UHost 或 UES的 Kibana
来实现。通过API可以删除通配符的一类索引或特定索引，也可以借助脚本来定时清理。

获取集群索引信息：

```
# UHost
curl -s -XGET 'http://<host>:9200/_cat/indices?v'

# Kibana
GET /_cat/indices?v
```

删除索引：

```
# UHost
curl -s -XDELETE 'http://<host>:9200/index'

# Kibana
DELETE /index
```

定时清理脚本（此脚本适用于logstash写入索引有时间标记的情况，其他情况可参考修改）：

```
#!/bin/bash
#author: UCloud
#created at 2017/08/30 19:00

if test ! -f "/var/log/elkDailyDel.log"; then
    touch /var/log/elkDailyDel.log
fi

# 获取索引信息
# 请将下行当中的localhost改成自己elasticsearch服务对应的Ip
indices=$(curl -s -XGET "localhost:9200/_cat/indices?v" |grep 'logstash' |awk '{print $3}')

# 设置保留索引的时间线
thirtyDaysAgo=$(date -d "$(date +%Y%m%d) -30 days" "+%s")
function DelOrNot(){
    if [ $(($1-$2)) -ge 0 ]; then
        echo 1
    else
        echo 0
    fi
}

for index in ${indices}
do
    indexDate=`echo ${index##*-} |sed 's/\./-/g'`
    indexTime=`date -d "${indexDate}" "+%s"`
    if [ `DelOrNot ${indexTime} ${thirtyDaysAgo}` -eq 0 ]; then
        # 索引时间在时间线之前执行删除操作
        # 请将下行当中的localhost改成你自己elasticsearch服务对应的Ip
        result=`curl -s -XDELETE "localhost:9200/${index}"`
        echo "delResult is ${result}" >> /var/log/elkDailyDel.log
        if [ `echo ${result} |grep 'acknowledged' |wc -l` -eq 1 ]; then
            echo "${index} had already been deleted!" >> /var/log/elkDailyDel.log
        else
            echo "there is something wrong happend when deleted ${index}" >> /var/log/elkDailyDel.log
        fi
    fi
done

echo $?
```

### 数据冷热分离

SSD磁盘类型的集群随着数据的不断写入，节点的磁盘数据逐渐增大。可能对于一些较老的数据，不需要太多的查询或不再做查询使用，那么这些数据就会占用大量的SSD性能资源和存储空间。数据冷热分离通过配置使最新的数据保存在ssd磁盘节点上，较老的数据自动迁移到廉价sata节点，使用有限的ssd节点资源来实现同时支持高并发读写和大数据量的存储。

UES也给出了数据冷热分离的策略，过程如下：

\* 准备

集群实例：

SSD（愿集群）：ues-qwerty，前三个节点ip分别为
ues-qwerty-ip1、ues-qwerty-ip2、ues-qwerty-ip3

SATA（新集群）：ues-asdfgh

1、愿集群（ues-qwerty）修改配置

控制台上修改 \*\* 参数管理 \*\* 中 \*\* node.attr.tag \*\* 项为 \*\* hot \*\*。

2、线性重启原集群（ues-qwerty）

3、固定原索引在原集群节点上（ues-qwerty）

```
# UHost
curl -XPUT localhost:9200/*/_settings -d '{"settings": {"index.routing.allocation.require.tag": "hot"}}'

# Kibana
PUT /*/_settings 
{ 
  "settings": { 
    "index.routing.allocation.require.tag": "hot"
  } 
}
```

4、新集群（ues-asdfgh）修改配置，同1

cluster.name: ues-qwerty

discovery.zen.ping.unicast.hosts:
\[ues-qwerty-ip1,ues-qwerty-ip2,ues-qwerty-ip3\]

node.attr.tag: cold

5、重启新集群（ues-asdfgh）

6、老数据迁移到 cold 节点

```
# UHost
curl -XPUT localhost:9200/cold_index/_settings -d '{"settings": {"index.routing.allocation.require.tag": "cold"}}'

# Kibana
PUT /cold_index/_settings 
{ 
  "settings": { 
    "index.routing.allocation.require.tag": "cold"
  } 
}
```

定时迁移脚本示例：

```
#!/bin/bash
#author: UCloud
#created at 2017/08/30 19:00

time=`date -d last-day "+%Y.%m.%d"`

# 请将下行当中的localhost改成自己elasticsearch服务对应的Ip
curl -XPUT http://localhost:9200/*-${time}/_settings -d'
{
  "index.routing.allocation.require.tag": "cold"
}'
```

**本章主要参考：**

\[《Elasticsearch:
权威指南》\](<https://www.elastic.co/guide/cn/elasticsearch/guide/current/administration.html>)
