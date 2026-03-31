# Dashboard

Dashboard是一个开源的分析与可视化平台，用于和OpenSearch一起使用的。你可以用Dashboard搜索、查看、交互存放在OpenSearch索引里的数据，使用各种不同的图表、表格、地图等kibana能够很轻易地展示高级数据分析与可视化。

Dashboard让我们理解大量数据变得更容易。产品默认安装Dashboard服务，你需要通过外网访问Dashboard，访问前需要先创建负载均衡，绑定Dashboard节点的IP地址，并将端口设置为5601后，在浏览器进行访问。具体操作步骤如下：

## 1.创建负载均衡实例

负载均衡创建方案可参考：[负载均衡创建指南](https://docs.ucloud.cn/ulb/NLB/guide/instance/create-instance)。

![image](/images/opensearch_new/dashboard01.png)


## 2.添加服务节点

进入刚刚创建的LB实例，在监听管理器中，添加节点；添加节点配置填写如下图：

![image](/images/opensearch_new/dashboard02.png)
![image](/images/opensearch_new/dashboard03.png)

## 访问Dashboar

节点添加完毕后，请在浏览器输入负载均衡的外网IP+5601端口访问Dashboard,注意前缀用“https://”：

![image](/images/opensearch_new/dashboard04.png)
![image](/images/opensearch_new/dashboard05.png)
