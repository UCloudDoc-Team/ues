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

![](/images/operate/backup/register_repository.png)

| 参数 | 说明 |
| ----- | ----- |
| UFile存储空间 | 用于存放UES集群的快照。<font color=red>注意：建立仓库之后不可以删除该仓库绑定的UFile存储空间，否则已经生成的快照文件将会丢失，新的快照也将无法生成。</font> |
| UFile令牌 | 用于授权访问上述存储空间，必须具有该存储空间的上传、下载、删除、文件列表权限，缺一不可。<font color=red>注意：建立仓库之后不可以删除该仓库绑定的UFile令牌，也不可以取消任何一项必备权限，同时还必须确保该UFile令牌未过有效期，否则快照生成、快照恢复等操作将会失败。</font> |
| 仓库名称 | 用于标识一个仓库，在一个UES集群内部不允许出现多个同名仓库。 |
| 基础路径 | 快照文件在存储空间中的路径，默认为ues\_backup，生成的快照文件会存放在UFile存储空间中的该目录下。 |
| 只读 | 控制UES集群对于该仓库的读写权限。<font color=red>注意：在多个UES集群中注册指向同一个路径（UFile存储空间 + 基础路径）的仓库时，只应该设置一个集群对其具有写入权限，其他集群应该设置为只读权限，以确保快照文件的完整性和一致性。</font> |
| 快照压缩 | 启用压缩后，将对快照中的索引映射和设置文件进行压缩，但数据文件不会被压缩。 |
| 分块大小 | 用于限制快照过程中单个文件分块的大小。 |
| 快照生成最大速度 | 用于限制每个节点生成快照的最大速度。 |
| 快照恢复最大速度 | 用于限制每个节点恢复快照的最大速度。 |
| 快照自动清理 | 如果开启快照自动清理，系统将会按照设定的时间长度，定期删除该仓库中过期的快照文件。 |

### 2.设定备份规则

在**“备份策略”**子页面中，点击**“新建规则”**按钮，在弹出的对话框中，按照页面提示完成相关信息的填写，点击确认，系统将为该集群创建一条自动生成快照的规则。

![](/images/operate/backup/create_snapshot_policy.png)

| 参数 | 说明 |
| ----- | ----- |
| 规则名称 | 用于标识一条备份规则。 |
| 快照名称 | 指定该备份规则所生成快照的名称，系统将在快照名称后添加唯一标识符以区分不同时间生成的快照。 |
| 所在仓库 | 选择用于存放快照的仓库，只能选择注册仓库时设置为具有写入权限的仓库。 |
| 执行计划 | 设定进行自动备份的频率和时间点，目前支持在每天的指定时间进行一次自动备份。 |
| 备份索引 | 默认备份全部索引，也可以输入索引名称以备份指定的索引，多个索引名称之间使用英文逗号分隔。 |
| 忽略不可用索引 | 用于控制对快照备份期间不可用索引的处理方式。此选项开启时，如果遇到不可用索引，将会继续生成不包含这些索引的快照。此选项关闭时，如果遇到不可用索引，快照备份将会失败。 |
| 允许不完全索引 | 用于控制对快照备份期间主分片不可用的索引的处理方式。此选项开启时，如果遇到此类情况，将会允许使用主分片不可用的索引生成快照。此选项关闭时，如果遇到此类情况，快照备份将会失败。 |
| 包含全局状态 | 用于控制是否将集群的全局状态保存在快照中。 |





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
