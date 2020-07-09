# 配置管理

**本章将列出可在控制台修改的静态配置项和可API动态更新的配置项。**

控制台支持修改常见**静态配置**参数到elasticsearch.yml配置文件。这些配置不支持动态更新，修改后需重启集群。


| 配置项 | 默认值 | 配置描述 |
| ----- | ----- | ----- |
| cluster.name | instance\\\_id | 集群名称 |
| discovery.zen.ping.unicast.hosts | \[\] | 单播地址，默认主节点IP或前三个节点IP，示例\[127.0.0.1,127.0.0.2,127.0.0.3\] |
| gateway.recover\\\_after\\\_nodes | N - 2 | 预期的节点数量达到，就可以进行恢复 |
| gateway.expected\\\_nodes | N | 预计集群节点数量。当预期的节点数量加入到群集，分片的恢复就会开始 |
| gateway.recover\\\_after\\\_time | 5m | 如果未达到预期的节点数量，则不会尝试恢复，恢复过程都会等待配置的时间量 |
| http.cors.enabled | true | 允许跨域访问 |
| http.cors.allow-origin | \* | 允许跨域访问的域，默认支持所有域 |
| rest.action.multi.allow\\\_explicit\\\_index | true | 允许Body中的index参数覆盖URL中的index参数 |
| node.attr.tag |   | 节点tag，默认不设置 |
| gateway.expected\\\_master\\\_nodes | 0 | 预计集群主节点数量。当预期的主节数量加入集群，分片的恢复就会开始 |
| gateway.expected\\\_data\\\_nodes | 0 | 预计集群数据节点数量。当预期的数据节数量加入集群，分片的恢复就会开始 |
| gateway.recover\\\_after\\\_master\\\_nodes | 0 | 预期的主节点数量达到，就可以进行恢复 |
| gateway.recover\\\_after\\\_data\\\_nodes | 0 | 预期的数据节点数量达到，就可以进行恢复 |
| http.max\\\_content\\\_length | 100mb | HTTP请求的最大内容，默认100mb。如果设置大于Integer.MAX\\\_VALUE，将被重置为100mb |
| http.max\\\_initial\\\_line\\\_length | 4kb | HTTP URL的最大长度。默认4kb |
| http.max\\\_header\\\_size | 8kb | 允许的headers的最大值。默认8kb |
| http.compression | true | 尽可能支持压缩（使用Accept-Encoding） |
| http.compression\\\_level | 3 | 定义HTTP响应的压缩级别。有效值在1（最小压缩）和9（最大压缩）的范围内 |
| http.cors.max-age | 1728000 | 浏览器发送"preflight"请求来确定CORS设置。max-age定义了结果应该被缓存的时间。默认为20天 |
| http.cors.allow-methods | OPTIONS,HEAD,GET,POST,PUT,DELETE | 允许的方法 |
| http.cors.allow-headers | X-Requested-With,Content-Type,Content-Length | 允许的headers |
| http.cors.allow-credentials | false | 是否应该返回Access-Control-Allow-Credentials header。注意：只有在设置为true的情况下才会返回此标题 |
| http.detailed\\\_errors.enabled | true | 在响应输出中启用或禁用详细错误消息和堆栈跟踪的输出。注意：如果设置为false，并指定了error\\\_trace请求参数，则会返回错误；当没有指定error\\\_trace时，会返回一个简单的消息 |
| http.pipelining | true | 启用或禁用HTTP流水线 |
| http.pipelining.max\\\_events | 10000 | 在HTTP连接关闭之前在内存中排队的最大事件数量 |
| indices.fielddata.cache.size | unlimited | 字段数据高速缓存的最大值，例如节点堆空间的30％，或绝对值，如12GB |
| indices.queries.cache.size | 10% | 控制过滤器缓存的内存大小，默认10％。 接受百分比值（如5％）或精确值（如512mb） |
| index.queries.cache.enabled | true | 控制是否启用查询缓存，设置是可以基于每个索引配置的索引设置 |
| indices.memory.index\\\_buffer\\\_size | 10% | 接受百分比或字节大小值。 默认10％，这意味着分配给节点的总堆的10％将被用作在所有分片之间共享的索引缓冲区 |
| indices.memory.min\\\_index\\\_buffer\\\_size | 48mb | 如果将index\\\_buffer\\\_size指定为百分比，则可以使用此设置指定绝对最小值 |
| indices.memory.max\\\_index\\\_buffer\\\_size | unlimited | 如果将index\\\_buffer\\\_size指定为百分比，则可以使用此设置指定绝对最大值 |
| indices.requests.cache.size | 1% | 缓存在节点级别进行管理，默认最大值为堆的1％ |
| node.ingest | true | 负载器资格 |
| search.remote.connect | true | 跨集群搜索 |
| index.number\\\_of\\\_shards | 5 | 索引应该具有的主分片的数量，此设置可在索引创建时动态设置 |
| index.shard.check\\\_on\\\_startup | false | 在分片打开之前是否检查有无损坏现象。当检测到损坏时，它将防止分片被打开 |
| index.routing\\\_partition\\\_size | 1 | 自定义路由值可以转到的分片数量。默认1，只能在索引创建时设置。此值必须小于index.number\\\_of\\\_shards，除非index.number\_of\_shards值也为1 |

  - **注：** N为节点个数

上面没有列出的配置项大多都可以通过
\*\*\_\[cluster-update-settings\](https:*www.elastic.co/guide/en/elasticsearch/reference/current/cluster-update-settings.html)\_\*\*
动态修改。动态配置项详细描述请参考\_\[Modules\](https:*www.elastic.co/guide/en/elasticsearch/reference/current/modules.html)\_

下面列出常见的可 **动态更新** 的配置参数。


| 配置项 | 默认值 | 配置描述 |
| ----- | ----- | ----- |
| cluster.routing.allocation.enable | all | 启用或禁用分配特定种类的分片 |
| cluster.routing.allocation.node\\\_concurrent\\\_incoming\\\_recoveries | 2 | 允许在一个节点上发生多少个并发传入分片恢复 |
| cluster.routing.allocation.node\\\_concurrent\\\_outgoing\\\_recoveries | 2 | 允许在一个节点上发生多少个并行传出分片恢复 |
| cluster.routing.allocation.node\\\_concurrent\\\_recoveries | 2 | node\\\_concurrent\\\_incoming\\\_recoveries和node\\\_concurrent\\\_outgoing\\\_recoveries快捷设置 |
| cluster.routing.allocation.node\\\_initial\\\_primaries\\\_recoveries | 4 | 启动节点后恢复未分配的主节点将使用本地磁盘中的数据 |
| cluster.routing.allocation.same\\\_shard.host | false | 允许执行检查单个主机上分配同一分片的多个实例 |
| cluster.routing.rebalance.enable | all | 启用或禁用特定种类的分片重新平衡 |
| cluster.routing.allocation.allow\\\_rebalance | indices\\\_all\\\_active | 指定何时允许分片重新平衡 |
| cluster.routing.allocation.cluster\\\_concurrent\\\_rebalance | 2 | 允许控制集群范围允许多少个并发分片重新平衡 |
| cluster.routing.allocation.balance.shard | 0.45f | 定义节点上分配的分片总数（浮点数）的权重因子 |
| cluster.routing.allocation.balance.index | 0.55f | 定义在特定节点上分配的每个索引的分片数（浮点数）的权重因子 |
| cluster.routing.allocation.balance.threshold | 1.0f | 应该执行的操作的最小优化值（非负浮点数） |
| cluster.routing.allocation.disk.threshold\\\_enabled | true | 设置为false可禁用磁盘分配决策程序 |
| cluster.routing.allocation.disk.watermark.low | 85％ | 控制磁盘使用的低水位，默认85％。超出ES就不会为节点分配新的分片 |
| cluster.routing.allocation.disk.watermark.high | 90% | 控制高水位，默认90％。超出ES将尝试将分片重定位到另一个节点 |
| cluster.routing.allocation.disk.watermark.flood\\\_stage | 95% | 控制洪水阶段，默认95％，超出ES强制在至少一个磁盘超出的节点上分配一个或多个分片的每个索引上的只读索引块。一旦有足够的磁盘空间可用于索引操作继续，索引块必须手动释放 |
| cluster.info.update.interval | 30s | ES应该检查集群中每个节点的磁盘使用情况 |
| cluster.routing.allocation.disk.include\\\_relocations | true | ES将在计算节点的磁盘使用情况时考虑当前正在重定位到目标节点的分片 |
| cluster.routing.allocation.awareness.attributes.\* |   | 分片分配感知设置允许您告诉ES您的硬件配置 |
| cluster.routing.allocation.include.{attribute} |   | 将分片分配给{属性}至少有一个逗号分隔值的节点，attribute 可能是\\\_name、\\\_ip、\\\_host |
| cluster.routing.allocation.require.{attribute} |   | 只将分片分配给{属性}具有所有逗号分隔值的节点，attribute 可能是\\\_name、\\\_ip、\\\_host |
| cluster.routing.allocation.exclude.{attribute} |   | 不要将分片分配给{属性}没有逗号分隔值的节点，attribute 可能是\\\_name、\\\_ip、\\\_host |
| cluster.blocks.read\\\_only | false | 使整个群集只读（索引不接受写操作），数据不允许被修改（创建或删除索引） |
| cluster.blocks.read\\\_only\\\_allow\\\_delete | false | 与cluster.blocks.read\\\_only相同，但允许删除索引以释放资源 |
| cluster.indices.tombstones.size | 500 | 集群状态维护索引墓碑以明确表示已被删除的索引 |
| logger.org.elasticsearch.indices.recovery | INFO | 日志记录级别 |
| discovery.zen.ping.unicast.hosts.resolve\\\_timeout | 5s | 在每轮ping之前等待DNS查找的时间 |
| discovery.zen.ping\\\_timeout | 3s | ping超时时间 |
| discovery.zen.join\\\_timeout | 60s | 默认超时时间是ping超时的20倍 |
| discovery.zen.minimum\\\_master\\\_nodes | N/2 + 1 | 最小主节点资格数，N为主节点个数 |
| discovery.zen.no\\\_master\\\_block | write | 设置控制在没有活动的主节点时应该拒绝哪些操作，默认write |
| indices.breaker.total.limit | 70% | 总体的parent breaker的起始限制，默认JVM堆的70% |
| indices.breaker.fielddata.limit | 60% | fielddata breaker的限制，默认JVM堆的60% |
| indices.breaker.fielddata.overhead | 1.03 | 一个常数，所有的fielddata估计相乘决定最后的估算 |
| indices.breaker.request.limit | 60% | request breaker的限制，默认JVM堆的60% |
| indices.breaker.request.overhead | 1 | 一个常数，所有的请求估计相乘决定最后的估算 |
| network.breaker.inflight\\\_requests.limit | 100% | inflight\\\_requests breaker的限制，默认JVM堆的100% |
| network.breaker.inflight\\\_requests.overhead | 1 | 一个常数，所有的飞行中的请求估计相乘决定最后的估算 |
| script.max\\\_compilations\\\_rate | 75/5m | 一定间隔内允许编译的唯一动态脚本的数量限制 |
| index.requests.cache.enable | true | 启用或禁用索引缓存 |
| indices.recovery.max\\\_bytes\\\_per\\\_sec | 40mb | 数据在节点间传输最大带宽 |
| indices.store.throttle.max\\\_bytes\\\_per\\\_sec | 20mb | 写磁盘最大带宽 |
| index.merge.scheduler.max\\\_thread\\\_count |   | 索引merge最大线程数，默认为max(1, min(4, availableProcessors / 2)) |
| index.number\\\_of\\\_replicas | 1 | 每个主分片具有的副本数量 |
| index.auto\\\_expand\\\_replicas | 默认false（即禁用） | 根据可用节点的数量自动扩展副本的数量。设置为划定短划线的下限和上限（例如0-5） |
| index.refresh\\\_interval | 1s | 多久执行一次刷新操作，这会使最近对索引进行的更改可见，从而进行搜索。可以设置为-1来禁用刷新 |
| index.max\\\_result\\\_window | 10000 | 搜索索引的from + size的最大值 |
| index.max\\\_inner\\\_result\\\_window | 100 | 内部匹配定义和顶部的from + size的最大值将聚合到此索引 |
| index.max\\\_rescore\\\_window | 10000 | 用于搜索此索引的重新调用请求的window\\\_size的最大值。默认index.max\\\_result\\\_window |
| index.max\\\_docvalue\\\_fields\\\_search | 100 | 查询中允许的最大docvalue\\\_fields数量 |
| index.max\\\_script\\\_fields | 32 | 查询中允许的script\\\_fields的最大数 |
| index.max\\\_ngram\\\_diff | 1 | NGramTokenizer和NGramTokenFilter的min\\\_gram和max\\\_gram的最大允许差值 |
| index.max\\\_shingle\\\_diff | 3 | max\\\_shingle\\\_size和min\\\_shingle\\\_size的最大允许差值 |
| index.blocks.read\\\_only | false | 设置为true以使索引和索引数据只读，否则允许写入和数据更改 |
| index.blocks.read\\\_only\\\_allow\\\_delete | false | 与index.blocks.read\\\_only相同，但允许删除索引以释放资源 |
| index.blocks.read | false | 设置为true以禁止对索引执行读取操作 |
| index.blocks.write | false | 设置为true以禁止对索引执行写入操作 |
| index.blocks.metadata | false | 设置为true以禁用索引数据读取和写入 |
| index.max\\\_refresh\\\_listeners |   | 索引的每个分片上可用的刷新监听器的最大数量。这些监听器用于实现refresh = wait\_for |