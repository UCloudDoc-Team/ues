# 创建Logstash实例

控制台上 **全部产品** 寻找 **大数据与中间件** 大类下 **[Elasticsearch Service 控制台](https://console.ucloud.cn/ues/ulogstash)** 进入产品页

## 创建
![image](/images/logstash/create_ulogstash_base_1.jpg)
![image](/images/logstash/create_ulogstash_type_1.jpg)
![image](/images/logstash/create_ulogstash_setting_1.jpg)

页面会列出可创建服务的全部地域和可用区，选择需求的可用区、服务版本、磁盘类型、节点类型、磁盘大小、节点数量、填写一些初始化信息等内容后创建就可实现服务实例的快速部署。

## 节点规格、节点数量选择以及付费方式

**其中可选的节点规格**

按磁盘、规格分类：

| 磁盘类型      | 标识   |
| ------- | ---- |
| RSSD云盘  | o.logstash |

可选节点规格详细配置（最新节点参考“UES价格”导航页）：

| 机型    | 名称           | 配置                 |
| ----- | ------------ | ------------------ |
| 内存密集型 | o.logstash4m.medium   | 2核 8G  |
| 内存密集型 | o.logstash4m.xlarge  | 4核 16G  |
| 内存密集型 | o.logstash4m.2xlarge | 8核 32G  |
| 内存密集型 | o.logstash4m.4xlarge | 16核 64G  |
| 普通机型  | o.logstash2m.medium  | 2核 4G  |
| 普通机型  | o.logstash2m.xlarge | 4核 8G  |
| 普通机型  | o.logstash2m.2xlarge  | 8核 16G  |
| 普通机型  | o.logstash2m.4xlarge  | 16核 32G |
| 普通机型  | o.logstash2m.8xlarge  | 32核 64G |

**计费选择**

计费模式分为按时、月付、年付3种类型。控制台支持计费类型修改。

## 确认支付

集群信息选择和填写完成后点击创建，到支付确认页面，完成支付创建。

## 实例部署

![image](/images/logstash/create_ulogstash_list_1.jpg)

等待部署中，由于实例规模不同，所需要的部署时间会有所差异，部署大致时间在2分钟到5分钟。

部署中，列表的实例的实例状态会显示为"蓝色"状态创建中，创建成功后实例状态会动态变化为"绿色"状态运行。

如果长时间为创建状态，可点击刷新重新获取实例状态。

