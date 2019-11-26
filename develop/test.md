

## ES测试

ES集群创建成功后（创建参考操作指南），可以进行集群可用性的初步测试。这里示例通过一台可以访问 ues 的 uhost
进行curl调用命令测试。

### 集群状态测试

```
curl -s -XGET 'http://<host>:9200/_cluster/health?pretty'
```

正常情况，系统返回结果：

``` curl
{
  "cluster_name" : "ues-qwerty",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 3,
  "number_of_data_nodes" : 3,
  "active_primary_shards" : 1,
  "active_shards" : 2,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
```

说明集群是正常的运行状态了，之后进行测试数据写入。

### 索引一个文档

例，索引为 "ucloud"，类型为 "information"，选择"1"作为ID的编号。这时，请求就是这样的：

```
curl -X PUT \
http://<host>:9200/ucloud/information/1 \
-H 'Content-Type: application/json' \
-d '{
    "name": "UCloud",
    "age": 5,
    "about": "at shanghai",
    "interests": ["service"]
}'
```

创建成功返回

``` curl
{"_index":"ucloud","_type":"information","_id":"1","_version":1,"result":"created","_shards":{"total":2,"successful":2,"failed":0},"created":true}
```

### 检索文档

```
curl -X GET 'http://<host>:9200/ucloud/information/1'
```

返回

``` curl
{"_index":"ucloud","_type":"information","_id":"1","_version":1,"found":true,"_source":{
    "name" : "UCloud",
    "age": 5,
    "about": "at shanghai",
    "interests": ["service"]
}}
```

### 搜索文档

```
curl -X GET 'http://<host>:9200/_search'
curl -X GET 'http://<host>:9200/ucloud/_search'
curl -X GET 'http://<host>:9200/ucloud/information/_search'
curl -X GET "http://<host>:9200/_sql" -H 'Content-Type: application/json' -d'select * from ucloud limit 10'
```


### 复杂搜索

复杂搜索需使用POST方法，详情请参考 功能文档 API文档

### 更新文档

```
curl -X PUT \
http://<host>:9200/ucloud/information/1 \
-H 'Content-Type: application/json' \
-d '{
    "name" : "UCloud",
    "age": 5,
    "about": "at shanghai",
    "interests": ["service", "professional"]
}'
```

更新成功返回

``` curl
{"_index":"ucloud","_type":"information","_id":"1","_version":2,"result":"updated","_shards":{"total":2,"successful":2,"failed":0},"created":false}
```

### 删除

删除文档

```
curl -X DELETE 'http://<host>:9200/ucloud/information/1'
```

删除指定类型文档

```
curl -X DELETE 'http://<host>:9200/ucloud/information'
```

删除索引

```
curl -X DELETE 'http://<host>:9200/ucloud'
```
