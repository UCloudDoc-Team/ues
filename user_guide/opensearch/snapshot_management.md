# 快照管理

## 前置说明
Opensearch快照管理分为三个环节：
1、添加快照密钥
2、创建仓库
3、创建快照

请按上述顺序依次执行，第一步添加快照密钥在UCloud控制台中操作，可参考[快照密钥管理文档](https://docs.ucloud.cn/ues/user_guide/opensearch/snapshot_key_management)；第二步创建仓库与第三步创建快照需登录OpenSearch的Dashboard中操作，可参考下文。

## 创建仓库

登录DashBoard（具体登录方法可参考[说明文档](https://docs.ucloud.cn/ues/user_guide/opensearch/dashboard)

左侧导航栏进入“Snapshot Management”

![images](/images/opensearch_new/snapshot/snapshot04.png)

进入“Snapshot Management”后，可在左侧选择“Repositories”进行仓库创建

![images](/images/opensearch_new/snapshot/snapshot05.png)

创建仓库时依次填写字段

![images](/images/opensearch_new/snapshot/snapshot06.png)

“Advanced settings”字段填写参考如下：

    {
      "type": "s3",
      "settings": {
       "endpoint": "internal.s3-cn-wlcb.ufileos.com",
       "protocol": "http",
       "client": "xxxxxx",
       "disable_chunked_encoding": true,
       "region": "cn-wlcb",
       "chunk_size": "50mb",
       "bucket": "xxxxxx",
       "base_path": "backed",
       "max_snapshot_bytes_per_sec": "20mb",
       "max_restore_bytes_per_sec": "20mb",
       "buffer_size": "8mb"
  }
}

其中，“endpoint”参考[文档](https://docs.ucloud.cn/ufile/introduction/region)中的内网域名；
“client”为快照密钥管理中所填写的值；
“region”参考[文档](https://docs.ucloud.cn/api/summary/regionlist)中的地域短ID

## 创建创建快照

创建完仓库后，昨天导航切换至“Snapshots”即可创建快照

![images](/images/opensearch_new/snapshot/snapshot07.png)

创建表单中的仓库即为上一步中创建的仓库

![images](/images/opensearch_new/snapshot/snapshot08.png)