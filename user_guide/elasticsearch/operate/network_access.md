# 公网访问

本文主要介绍通过公网访问Kibana、公网访问ES实例、以及ES通过公网进行外部访问。


## 1.通过公网访问Kibana

此部分内容可参考：[Kibana文档](elasticsearch\operate\kibana.md)。


## 2.通过公网访问ES实例

### 2.1 创建负载均衡实例

负载均衡创建方案可参考：[负载均衡创建指南](https://docs.ucloud.cn/ulb/NLB/guide/instance/create-instance)。

![image](/images/opensearch_new/dashboard01.png)


### 2.2 添加服务节点

进入刚刚创建的LB实例，在监听管理器中，添加节点；添加节点配置填写如下图：

![image](/images/opensearch_new/dashboard02.png)
![image](/images/network_access\004.png)

### 2.3 访问实例

节点添加完毕后，请在浏览器输入负载均衡的外网IP+9200端口访问实例,注意前缀用“https://”：

## 3.ES通过公网进行外部访问

进入“私有网络 VPC产品”，并切换至“NAT网关”tab进行创建网关

![image](/images/network_access\002.png)

创建NAT网关时，VPC与子网选择和ES相同，创建成功后， ES实例即拥有公网访问能力。

![image](/images/network_access\003.png)