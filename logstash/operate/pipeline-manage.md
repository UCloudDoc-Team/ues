# 管道管理
Logstash 通过管道来实现数据的采集处理，它包含必选的 input 和 output 插件，以及可选的 filter 插件，并支持多管道并行运行，。本文介绍如何通过配置文件管理管道，包括创建管道、修改管道和删除管道。

## 创建管道
1. 登录 [Elasticsearch Service 控制台](https://console.ucloud.cn/ues/ulogstash)，进入 Logstash 实例列表页。

2. 在实例列表页，单击实例名称进入实例详情页，然后进入管道管理页签，单击创建管道。
![创建管道1](/images/logstash/create_ulogstash_pipeline_1.png)

3. 填写管道名称（支持字母、数字、_）
![创建管道2](/images/logstash/create_ulogstash_pipeline_2.png)

4.  Config配置

    用户根据任务内容进行修改。
    ![创建管道3](/images/logstash/create_ulogstash_pipeline_3.png)
    **参数说明**
    | 参数   | 说明                                     |
    | ------ | ---------------------------------------- |
    | input  | 输入数据源配置。Logstash 支持的输入数据源类型，可参考 [Input plugins](https://www.elastic.co/guide/en/logstash/7.10/input-plugins.html) |
    | filter | 对数据进行过滤或者预处理的配置。Logstash 支持的 filter 插件类型，可参考 [Filter plugins](https://www.elastic.co/guide/en/logstash/7.10/filter-plugins.html) |
    | output | 输出数据源配置。Logstash 支持的输出数据源类型，可参考 [Output plugins](https://www.elastic.co/guide/en/logstash/7.10/output-plugins.html) |
    管道任务配置详情可参考 [配置文件结构](https://www.elastic.co/guide/en/logstash/7.10/configuration-file-structure.html)。

5. 管道参数配置

    | 参数              | 说明                                                      | 默认值     |
    | ----------------- | --------------------------------------------------------- | ---------- |
    | 管道名称           | pipeline.id，管道的唯一标识                                | -          |
    | 管道工作线程     | pipeline.workers，管道的工作线程数量，也是并行执行管道的 filter 和 output 的工作线程数量 | 实例单节点的 CPU 核数 |
    | 管道批处理大小   | pipeline.batch.size，每个批次处理的最大事件数量           | 125        |
    | 管道批处理延迟   | pipeline.batch.delay，当管道批处理大小不满足时，每个批次最大的等待时间，单位为毫秒 | 50ms       |
    | 队列类型          | queue.type，用于事件缓冲的排队模型，可选值为 memory（基于内存的内列）。暂不支持修改 | memory     |
    | 队列最大字节数   | queue.max_bytes，当选择 persisted 队列类型时，队列中可存放的最大字节数量，需确保该值小于实例单节点的磁盘容量。 暂不支持修改 | 1024MB     |
    | 队列检查点写入数 | queue.checkpoint.acks，当选择 persisted 队列类型时，在强制执行检查点时已写入的最大的事件数量，若设置为0，则表示无限制。 暂不支持修改 | 1024       |

## 修改管道
> 管道修改后需要进行重启才能生效

1. 在管道列表中，点击要修改的管道右侧的修改按钮，进入管道修改页面。
2. 在管道修改页面修改管道的任务配置和参数配置。
3. 点击保存，完成管道修改，修改后的内容将在管道下次启动时生效。

## 删除管道
> 注：
> - 管道删除后无法恢复
> - 运行中的管道需要先停止后才能执行删除

1. 在管道列表中，找到需要删除的管道，在右侧操作列表中点击删除。