{{indexmenu_n>2}}

## 集群信息

控制台会给出集群的基本信息展示。点击详情进入详情页，我们会提供集群概况、节点信息和集群监控视图等信息。

### 基本信息

![image](/images/operate/detail_baseinfo_1.jpg)

其中资源名（和列表中 "集群名称"
完全相同）可以自定义，如上图标注。请注意此资源名不是Elasticsearch配置文件中的集群名称（配置文件中的
cluster.name 默认为 \*\* 资源ID \*\*）。

### 节点配置

![image](/images/operate/detail_configinfo_1.jpg)

集群节点配置详情。

如果集群是主节点分离类型，标题为数据节点配置，主节点配置不在此栏中显示。

### 付费信息

![image](/images/operate/detail_chargeinfo_1.jpg)

集群付费详情。续费可以到计费系统进行续费、更改计费方式等操作。

### 集群健康

![image](/images/operate/detail_cluster_health_1.jpg)

该部分就是API \`/\_cluster/health\` 的展示信息，可以提供简单的集群状态。服务关闭状态，该部分显示空内容。

该部分展示了最基础的集群状态、分片数量、正在迁移分片数、未分配分片数等重要集群信息。

### 节点列表

![image](/images/operate/detail_nodelist_1.jpg)

展示集群节点IP信息、节点配置信息及节点可用操作。

节点IP为业务网地址，**ues默认是通过内网访问的，为了数据安全性，强烈不建议代理到外网环境访问**。

**节点类型分为mi、di、mdi 3种**。

mi和di为主节点分离类型集群显示，mi为主节点资格节点，di为数据节点。

mdi为基础类型集群显示，mdi为节点具备主节点资格并可以用来保存数据。

具体查看哪一个节点为主节点，可自行通过API \`/\_cat/master?v\` 获取。
