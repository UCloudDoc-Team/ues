# 索引管理

通过索引管理可以快速实现索引配置、缓存清理操作。本文介绍如何管理索引。

## 集群索引

进入集群索引页面，可以获取当前集群索引信息列表。

操作流程

> 1. 登录控制台集群[列表页](https://console.ucloud.cn/ues/manage)
> 2. 找到目标集群，点击操作列的“详情”按钮，进入集群详情页面
> 3. 点击“索引管理”页签，进入索引管理
> 4. 点击“集群索引”页签，进入集群索引

![image](/images/index_manage_1.png)

## 缓存清理

### 清理全部缓存

![image](/images/index_manage_2.png)

清理全部缓存, 相当于执行在Kibana 中执行 **POST _cache/clear** 命令。

### 清理缓存

![image](/images/index_manage_3.png)

清理索引缓存操作，相当于执行在Kibana Dev Tools 中执行 **POST ${index_name}/_cache/clear** 命令； 即清理选中索引缓存。

## 索引配置

![image](/images/index_manage_4.png)

查看选中索引的配置。

![image](/images/index_manage_5.png)

编辑索引配置中的 "mappings","settings"，其内容于在Kibana Dev Tools 修改索引配置中 "_mappings","_settings" 一致。

例如修改索引配置为：
```
{
  "mappings": {
    "properties": {
      "summary": {
        "type": "text",
        "analyzer": "english"
      }
    }
  },
  "settings": {
    "index": {
      "refresh_interval": "5s",
      "number_of_replicas": "1"
    }
  }
}
```

等价于在Kibana Dev Tools 执行以下语句：

```
PUT ${index_name}/_mappings
{
  "properties": {
    "summary": {
      "type": "text",
      "analyzer": "english"
    }
  }
}
```

```
PUT ${index_name}/_settings
{
    "index.refresh_interval": "5s",
    "index.number_of_replicas": 1
}
```

## 分片分布

![image](/images/index_manage_6.png)

查看索引分片在节点的分布情况。


