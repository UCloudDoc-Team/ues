# US3插件

US3插件支持将Elasticsearch数据的快照备份至US3，并可从已备份至US3的快照中将数据恢复至Elasticsearch。

## 一、安装插件

可以通过UES控制台的“插件管理”功能进行US3插件的安装，插件名称为elasticsearch-repository-ufile，具体操作方法请参考文档[插件管理](ues/plugins/manage)章节。

## 二、创建US3存储空间

### 1.创建存储空间

在US3控制台创建用于UES数据备份的存储空间，为了确保网络传输质量，请选择**与目标UES集群相同的地域**进行创建。空间类型请选择**“私有空间”**。
![](/images/plugins/us3/01-创建us3存储空间.png)

### 2.创建令牌

在US3控制台创建用于访问上述存储空间的令牌，请注意选择上述创建的存储进行授权，并在“令牌权限”中勾选“上传”、“下载”、“删除”、“文件列表”等。
![](/images/plugins/us3/02-创建us3令牌.png)

### 3.获取存储空间内网域名及密钥信息

获取并记录以下信息以备使用：

**（1）存储空间内网域名**

请注意US3控制台展示的存储空间域名是外网域名，您需要查看US3文档获取其内网域名，详见US3文档的[“产品简介” - “地域和域名”](/ufile/introduction/region)一节。

以上述创建的示例存储空间为例，其外网域名为：ues-backup.uae-dubai.ufileos.com，查询得到US3迪拜的内网域名为：www.internal-uae-dubai.ufileos.com ，故示例存储空间的内网域名为：**ues-backup.internal-uae-dubai.ufileos.com**
![](/images/plugins/us3/03-获取存储空间域名.png)

**（2）拥有上述存储空间访问权限的令牌密钥**

在US3控制台“令牌管理”页面查看令牌的公钥和私钥。
![](/images/plugins/us3/04-获取令牌密钥.png)

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

\<2\> endpoint：上述US3存储空间的内网域名

\<3\> public\_key：上述US3令牌的公钥

\<4\> private\_key：上述US3令牌的私钥

\<5\> bucket：上述US3存储空间的名称

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