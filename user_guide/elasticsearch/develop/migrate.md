# 实例迁移

## 创建索引

数据迁移本质是索引的重建，重建索引不会尝试设置目标索引，它不会复制源索引的设置。 所以在操作之前设置目标索引，包括设置映射，分片数，副本等。

## 数据迁移

### 1\. Reindex from Remoteedit

Reindex支持从远程Elasticsearch集群重建索引：

```
POST _reindex
{
  "source": {
    "remote": {
      "host": "http://otherhost:9200",
      "username": "user",
      "password": "pass"
    },
    "index": "source",
    "query": {
      "match": {
        "test": "data"
      }
    }
  },
  "dest": {
    "index": "dest"
  }
}

# host参数必须包含scheme、host和port（例如https:// otherhost：9200）
# username和password参数可选
```

使用时需要在elasticsearch.yml中配置 reindex.remote.whitelist
属性。可以设置多组（例如，otherhost:9200, another:9200,
127.0.10.\*:9200, localhost:\*）。

具体使用可参考 _**[Reindex from
Remoteedit](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html#reindex-from-remote)**_

### 2\. Elasticsearch-Dump

Elasticsearch-Dump是一个elasticsearch数据导入导出开源工具包。安装、迁移相关执行可以在相同可用区的云主机上进行，使用方便。

* **Installing**

需要node环境，npm安装elasticdump

```
npm install elasticdump -g
elasticdump
```

* **Use**

```
# Copy an index from production to staging with analyzer and mapping:
elasticdump \
  --input=http://production.es.com:9200/my_index \
  --output=http://staging.es.com:9200/my_index \
  --type=analyzer
elasticdump \
  --input=http://production.es.com:9200/my_index \
  --output=http://staging.es.com:9200/my_index \
  --type=mapping
elasticdump \
  --input=http://production.es.com:9200/my_index \
  --output=http://staging.es.com:9200/my_index \
  --type=data

# Copy a single shard data:
elasticdump \
  --input=http://es.com:9200/api \
  --output=http://es.com:9200/api2 \
  --params='{"preference" : "_shards:0"}'
```

elasticdump命令其他参数使用参考 _**[Elasticdump
Options](https://github.com/taskrabbit/elasticsearch-dump#options)**_

### 3\. Elasticsearch-Migration

Elasticsearch-Migration是基于Go语言开源的工具包。和Elasticsearch-Dump一样，相关操作可以在相同可用区的云主机上进行。

* **Download**

_**[下载地址](<https://github.com/medcl/esm/releases>)**_

* **Use**

```
./bin/esm -s http://192.168.1.x:9200/ -d http://192.168.1.y:9200/ -x src_index -y dest_index -w=5 -b=100

# Options
  -s, --source=     source elasticsearch instance
  -d, --dest=       destination elasticsearch instance
  -q, --query=      query against source elasticsearch instance, filter data before migrate, ie: name:medcl
  -m, --source_auth basic auth of source elasticsearch instance, ie: user:pass
  -n, --dest_auth   basic auth of target elasticsearch instance, ie: user:pass
  -c, --count=      number of documents at a time: ie "size" in the scroll request (10000)
  --sliced_scroll_size=      size of sliced scroll, to make it work, the size should be > 1, default:"1"
  -t, --time=       scroll time (1m)
      --shards=     set a number of shards on newly created indexes
      --copy_settings copy index settings from source
      --copy_mappings copy mappings mappings from source
  -f, --force      delete destination index before copying, default:false
  -x, --src_indexes=    list of indexes to copy, comma separated (_all), support wildcard match(*)
  -y, --dest_index=    indexes name to save, allow only one indexname, original indexname will be used if not specified
  -a, --all         copy indexes starting with . and _ (false)
  -w, --workers=    concurrency number for bulk workers, default is: "1"
  -b  --bulk_size   bulk size in MB" default:5
  -v  --log         setting log level,options:trace,debug,info,warn,error
  -i  --input_file  indexing from local dump file, file format: {"_id":"xxx","_index":"xxx","_source":{"xxx":"xxx"},"_type":"xxx"  }
  -o  --output_file output documents of source index into local file, file format same as input_file.
  --source_proxy     set proxy to source http connections, ie: http://127.0.0.1:8080
  --dest_proxy       set proxy to destination http connections, ie: http://127.0.0.1:8080
  --refresh          refresh after migration finished
```

Elasticsearch-Migration注意事项 **[参考链接](https://github.com/medcl/esm)**