# 数据备份

UES的数据备份功能，使用Elasticsearch（以下简称ES）的 snapshot API，通过[elasticsearch-repository-ufile](https://github.com/ufilesdk-dev/elasticsearch-repository-ufile)插件（以下简称UFile插件）的支持，按照预先设定的备份计划，定时自动生成快照，保存在[对象存储 UFile](https://docs.ucloud.cn/ufile/README)当中，从而实现对UES索引数据的有效备份。

## 名词释义

## 准备工作

### 1.创建UFile存储空间

登录[UFile控制台](https://console.ucloud.cn/ufile/ufile)，创建用于保存UES快照数据的存储空间。要求：
（1）存储空间所在地域必须与您需要快照备份的UES集群所在地域保持一致。
（2）空间类型请选择**”私有空间“**，以确保存储空间内的数据需要得到密钥授权才能访问。
![](/images/operate/backup/create_ufile_bucket.png)

### 2.创建UFile令牌

在[令牌管理](https://console.ucloud.cn/ufile/token)页面，创建用于授权访问上述存储空间的令牌，合理设置有效时长，授权访问上述存储空间，并赋予对**所有文件**的**上传、下载、删除、文件列表权限**。
![](/images/operate/backup/create_ufile_token.png)

### 3.为需要备份的UES集群安装UFile插件

登录[UES控制台](https://console.ucloud.cn/ues/manage)，在目标UES集群的插件管理页面进行UFile插件的安装，具体操作方法可以参考文档[插件管理](/ues/plugins/manage)章节。
![](/images/operate/backup/install_ufile_plugin.png)

## 操作步骤

数据备份功能入口：登录[UES控制台](https://console.ucloud.cn/ues/manage)，点击集群列表中目标集群的“详情”按钮，进入集群详情页面，切换至“数据备份”标签页。

### 1.注册仓库

在**“仓库管理”**子页面中，点击**“注册仓库”**按钮，在弹出的对话框中，按照页面提示完成相关信息的填写，点击确认，系统将为该集群创建一个存放快照的仓库。

| 参数 | 说明 |
| UFile存储空间 | <font color=red>注意：</font> |





--------------------------------------------------------------------



## 三、创建仓库

在对ES数据创建快照或者从快照恢复数据之前，需要先创建一个仓库。

    PUT /_snapshot/<1>
    {
        "type": "ufile",
        "settings": {
            "endpoint": <2>,
            "public_key": <3>, 
            "private_key": <4>, 
            "bucket": <5>,
            "compress": <6>,
            "chunk_size": <7>,
            "base_path": <8>,
            "max_snapshot_bytes_per_sec": <9>,
            "max_restore_bytes_per_sec": <10>
        }
    }

\<1\>：ES备份仓库的名称

\<2\> endpoint：上述UFile存储空间的内网域名

\<3\> public\_key：上述UFile令牌的公钥

\<4\> private\_key：上述UFile令牌的私钥

\<5\> bucket：上述UFile存储空间的名称

\<6\> compress：是否对备份的索引进行压缩（true / false）

\<7\> chunk\_size：上传文件的分片大小，默认为64MB

\<8\> base\_path：备份文件在bucket中的路径（前缀名称），默认为根路径（无前缀）

\<9\> max\_snapshot\_bytes\_per\_sec：生成快照时的上传速度，默认40MB/s

\<10\> max\_restore\_bytes\_per\_sec：从快照恢复时的下载速度，默认40MB/s

一个典型的仓库创建请求示例如下：

    PUT /_snapshot/ues_snapshot
    {
        "type": "ufile",
        "settings": {
            "endpoint": "ues-backup.internal-uae-dubai.ufileos.com",
            "public_key": "TOKEN_XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", 
            "private_key": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", 
            "bucket": "ues-backup",
            "compress": true,
            "chunk_size": "50mb",
            "base_path": "ues",
            "max_snapshot_bytes_per_sec": "20mb",
            "max_restore_bytes_per_sec": "20mb"
        }
    }

## 四、使用方法

关于创建快照、从快照恢复数据、查询快照信息等操作的具体方法，请参考Elastic官方文档：

[Snapshot And
Restore](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html)

## 五、常用命令

查询所有仓库信息：

    GET /_snapshot?pretty

查询一个仓库信息：

    GET /_snapshot/<repository>?pretty

查询一个仓库中所有快照：

    GET /_snapshot/<repository>/_all?pretty

查询一个指定快照：

    GET /_snapshot/<repository>/<snapshot>?pretty

创建一个快照：

    PUT /_snapshot/<repository>/<snapshot>

创建指定索引的一个快照：

    PUT /_snapshot/<repository>/<snapshot>
    {
        "indices": "<index1>[,index2]..."
    }

恢复一个指定快照：

    POST /_snapshot/<repository>/<snapshot>/_restore

删除一个指定快照：

    DELETE /_snapshot/<repository>/<snapshot>
