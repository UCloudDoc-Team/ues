# 释放实例

当 Logstash 实例不再需要时或无法满足您的需求，您可以在 Elasticsearch服务 控制台对实例进行释放，以避免服务继续运行而产生费用。

## 注意事项

实例释放后数据无法恢复，实例中所包含的管道会被删除，请谨慎操作。

## 操作步骤

1. 登录 **Elasticsearch服务 UES 控制台**，进入ULogstash 实例列表页。

![image](/ulogstash/images/ulogstash_release_clusterlist_1.jpg)

2. 在实例列表页，选择需要释放的实例，选择操作 > ... > 删除集群；或单击实例 ID/名称进入实例基本信息页，选择左上角删除集群。

![image](/ulogstash/images/ulogstash_release_cluster_1.jpg)

3. 在删除集群页面中，点击确定，系统将清空实例数据，并回收资源，数据清空后，无法恢复。