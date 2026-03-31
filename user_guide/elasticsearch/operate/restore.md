# 数据恢复

如果使用UES的[数据备份](/ues/operate/backup)功能生成了集群的快照，则可以使用Elasticsearch（以下简称ES）的 snapshot API，将快照中的数据恢复到指定的UES集群中。（下文中描述的数据恢复方式，对于通过其他途径生成的UES集群快照也适用。）

## 准备工作

### 1.确认快照仓库能否正常访问

如果需要从使用UES的数据备份功能生成的快照中恢复数据，必须确保以下几点：

（1）注册仓库时绑定的US3存储空间可以正常访问，且存放在其中的快照文件完好。

（2）注册仓库时绑定的用于授权访问上述US3存储空间的令牌状态正常，即自仓库创建之后该令牌未被删除过，具有上传、下载、删除、文件列表等各项必备权限，且当前该令牌处于有效期内。

### 2.确认快照的版本与目标UES集群的版本是否兼容

由于不同版本UES集群的索引数据结构可能存在差异，因此快照中的索引数据只能被恢复到能够正常识别和读取这些索引的UES集群中。关于快照版本和集群版本的兼容性，Elastic官方给出的基本规则是：版本相同时可以正常恢复，跨版本情况下低版本集群创建的快照可以被恢复到大版本号差异不超过 1 的高版本集群中，即：

（1）在 6.x 版本集群中创建的快照可以被恢复到 7.x 版本的集群中。

（2）在 5.x 版本集群中创建的快照可以被恢复到 6.x 版本的集群中。

（3）在 2.x 版本集群中创建的快照可以被恢复到 5.x 版本的集群中。

（4）在 1.x 版本集群中创建的快照可以被恢复到 2.x 版本的集群中。

除此以外，无法将高版本集群创建的快照恢复到低版本集群中，也无法将低版本集群创建的快照恢复到跨 2 个或者更多的大版本号的高版本集群中。

使用UES的数据备份功能生成的快照的版本信息，可以通过[UES控制台](https://console.ucloud.cn/ues/manage)：**数据备份 -\> 快照管理 -\> 快照详情**查看，或者接下来介绍的**“查看指定仓库中的所有快照”**命令查看。

## 操作步骤

下文中描述的操作步骤，均在UES集群的**Kibana控制台**中进行。功能入口：登录[UES控制台](https://console.ucloud.cn/ues/manage)，点击集群列表中目标集群右侧的“打开Kibana”按钮，进入**Kibana控制台**，点击左侧导航栏的 **Dev Tools** ，在 **Console** 框中输入并执行相应命令。

**注意：下列命令样例中尖括号 \<\> 里的内容，需要根据自身情况，替换成相应的内容（去掉尖括号）。**

### 1.查看当前集群的所有快照仓库

命令样例：

    GET /_snapshot/_all

命令返回结果样例：

![](/images/operate/restore/sample_get_all_repositories.png)

| 参数 | 说明 |
| ----- | ----- |
| my_first_repository | 仓库名称，将在后续的命令中用到。 |
| type | 仓库存储系统的类型，“ufile”表示使用的是对象存储US3。 |
| bucket | 仓库所在的US3存储空间名称。 |
| base_path | 快照在上述US3存储空间中的存放路径。 |

### 2.查看指定仓库中的所有快照

命令样例：

    GET /_snapshot/<my_first_repository>/_all

命令返回结果样例：

![](/images/operate/restore/sample_get_all_snapshots.png)

| 参数 | 说明 |
| ----- | ----- |
| snapshot | 快照名称，将在后续的命令中用到。 |
| version | 快照版本，可用于判断与本集群的兼容性。 |
| indices | 快照中包含的索引，恢复时可以指定只恢复其中的部分索引。 |

### 3.从指定快照中恢复索引数据

**注意：如果待恢复的索引在目标集群中已经存在，需要确保该索引已经被关闭，且与快照中的同名索引具有相同的分片数，否则恢复操作无法执行。**

命令样例：

    POST /_snapshot/<my_first_repository>/<daily_backup_202003281600>/_restore

命令返回结果样例：

![](/images/operate/restore/sample_restore.png)

由于该命令为异步命令，返回上述信息时，数据恢复任务将在后台执行。如果希望监测数据恢复进程，可以在上述命令后增加 **wait\_for\_completion** 参数，这将阻塞客户端直至操作完成后返回信息。

带有更多参数的命令样例：

    POST /_snapshot/<my_first_repository>/<daily_backup_202003281600>/_restore?wait_for_completion=true
    {
        "indices": "index_2,index_3",
        "ignore_unavailable": true,
        "include_global_state": true,
        "rename_pattern": "index_(.+)",
        "rename_replacement": "restored_index_$1"
    }

| 参数 | 说明 |
| ----- | ----- |
| indices | 指定需要恢复的快照名称，支持多索引语法。 |
| ignore_unavailable | 控制是否忽略不可用的索引。 |
| include_global_state | 控制是否恢复集群的全局状态。 |
| rename_pattern | 使用正则表达式匹配需要重命名的索引。 |
| rename_replacement | 指定重命名规则。 |

该命令返回结果样例：

![](/images/operate/restore/sample_restore_with_parameters.png)

## 参考信息

关于创建快照、从快照恢复数据、查询快照信息等操作的更多信息，请参考Elasticsearch官方文档：

[Snapshot And Restore](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshot-restore.html)