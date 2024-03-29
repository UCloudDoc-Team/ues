# 实例改配

随着业务的发展，您可能对集群配置的要求有变化。当集群的配置无法满足业务需求时，可通过集群改配功能，更改集群配置。

## 注意事项

改配集群会触发集群重启，重启时间与集群规格、数据结构和大小等因素有关，建议在业务低峰期操作。

## 操作步骤

1. 登录 [ULogstash 控制台](https://console.ucloud.cn/ues/ulogstash)。

2. 在顶部菜单栏处，选择地域。

3. 单击ULogstash管理，然后在ULogstash集群列表中单击目标集群名称。

4. 在节点信息页面，单击右侧的编辑节点。

5. 在编辑节点页面，选择相应的修改类型。

（1）增加节点，新增相同配置的节点。

 ![image](/images/logstash/resize_ulogstash_node_1.png)

（2）调整规格，修改当前节点配置、节点重启生效。

 ![image](/images/logstash/resize_ulogstash_type_1.png)

（3）修改磁盘大小，磁盘只允许扩大。

 ![image](/images/logstash/resize_ulogstash_disk_1.png)

6. 选中对应修改配置，单击立即购买。购买后，集群会完成相应的配置更改。
