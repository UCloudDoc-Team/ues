# 用户管理
UES 安全用户创建、修改、删除等相关功能。

## 一：预定义操作组

### General

| Name      | Description                                                                            |
| ----------- | ---------------------------------------------------------------------------------------- |
| UNLIMITED | Grants complete access, can be used on index- and cluster-level. Equates to `"*"`. |

### Index-level action groups

| Name            | Description                                                                                                             |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| INDICES_ALL     | Grants all permissions on the index. Equates to `indices:*`                                                         |
| GET             | Grants permission to use get and mget actions only                                                                      |
| READ            | Grants read permissions like get, mget or getting field mappings, and search permissons                                 |
| WRITE           | Grants write permissions to documents                                                                                   |
| DELETE          | Grants permission to delete documents                                                                                   |
| CRUD            | Combines the READ, WRITE and DELETE action groups                                                                       |
| SEARCH          | Grants permission to search documents. Includes SUGGEST.                                                                |
| SUGGEST         | Grants permission to use the suggest API. Already included in the READ action group.                                    |
| CREATE_INDEX    | Grants permission to create indices and mappings                                                                        |
| INDICES_MONITOR | Grants permission to execute all actions regarding index monitoring, e.g. recovery, segments info, index stats & status |
| MANAGE_ALIASES  | Grants permission to manage aliases                                                                                     |
| MANAGE          | Grants all `monitor` and index administration permissions                                                           |

### Cluster-level action groups

| Name                     | Description                                                                                                             |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| CLUSTER_ALL              | Grants all cluster permissions. Equates to `cluster:*`                                                              |
| CLUSTER_MONITOR          | Grants all cluster monitoring permissions. Equates to `cluster:monitor/*`                                           |
| CLUSTER_COMPOSITE_OPS_RO | Grants read-only permissions to execute multi requests like mget, msearch or mtv, plus permission to query for aliases. |
| CLUSTER_COMPOSITE_OPS    | Same as `CLUSTER\_COMPOSITE\_OPS\_RO`, but also grants bulk write permissions and all aliases permissions.          |
| MANAGE_SNAPSHOTS         | Grants full permissions to manage snapshots and repositories.                                                           |

## 二：创建用户

1. **elastic** 为默认用户，即创建集群时Kibana用户、密码 

2. **创建用户**

![image](/images/security/manager_create_user_1.png)

集群权限和索引权限可以分别设置；索引权限需设置 **Index Pattern** 配合操作组使用。

## 三：修改用户

1. **权限修改**

![image](/images/security/manager_update_user_1.png)

可修改集群权限和索引权限

2. **重置密码**

![image](/images/security/manager_reset_password_1.png)

## 四：删除用户

![img.png](/images/security/manager_delete_user_1.png)

默认用户不可删除

