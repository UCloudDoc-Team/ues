# 创建集群

登录控制台，在 **产品与服务** 寻找 **大数据** 大类下 **云搜索服务 CSS** 进入[产品创建页](https://console.ucloud.cn/ues/create)。

页面会列出可创建服务的全部地域和可用区，选择需求的可用区、服务版本、磁盘类型、节点类型、磁盘大小、节点数量、填写一些初始化信息等内容后创建就可实现服务集群的快速部署。

## 地域及基础配置

创建集群时需先选择部署地域及可用区，OpenSearch一期仅在香港地域上线。
![image](/images/opensearch_new/create01.png)

## 服务版本

服务版本选择 **OpenSearch** 的引擎版本
![image](/images/opensearch_new/create02.png)

## 节点配置

### 数据节点

![images](/images/opensearch_new/create03.png) 


### Dashboard节点

![images](/images/opensearch_new/create04.png) 


### 专有主节点

![images](/images/opensearch_new/create05.png) 

### 协调节点

![images](/images/opensearch_new/create06.png) 

## 网络设置

![images](/images/opensearch_new/create07.png) 


## Dashboard账号密码设置

Dashboard是一个开源的分析与可视化平台，是Elasticsearch比较常用的管理工具。您可以用kibana搜索、查看、交互集群中的索引数据，使用各种不同的图表、表格、地图等，dashboard都能够很轻易地展示高级数据分析与可视化。

![images](/images/opensearch_new/create08.png) 



## 后续操作

1. 确认支付：集群信息选择和填写完成后点击创建，到支付确认页面，完成支付创建。
2. 集群部署：等待部署中，由于集群规模不同，所需要的部署时间会有所差异，部署大致时间在2分钟到5分钟。部署中，列表的集群的集群状态会显示为"蓝色"状态创建中，创建成功后集群状态会动态变化为"绿色"状态运行。如果长时间为创建状态，可点击刷新重新获取集群状态。

![images](/images/opensearch_new/create09.png) 

