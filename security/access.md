# 客户端访问安全用户集群

## FileBeat
使用 filebeat-oss-6.8.4 版本，配置：
```
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/*.log

output.elasticsearch:
  hosts: ["xx.xx.xx.xx:9200"]
  username: "elastic"
  password: "changeme"
```
## Logstash
使用 logstash-oss-6.8.4 版本，配置：
```
input {
      generator {
        lines => [
          "line 1",
          "line 2",
          "line 3"
        ]
        # Emit all lines 3 times.
        count => 3
      }
    }

output {
      elasticsearch {
        hosts => ["xx.xx.xx.xx:9200"]
        user => "elastic"
        password => "changeme"
      }
    }
```

## Go Lang
```
package main

import "fmt"
import "log"
import "github.com/elastic/go-elasticsearch/v6"

func main() {
    fmt.Println("Hello, World!")
   cfg := elasticsearch.Config{
                Addresses: []string{"http://xx.xx.xx.xx:9200"},
                Username: "elastic",
                Password: "changeme",
        }

    es,_ := elasticsearch.NewClient(cfg)
    log.Println(elasticsearch.Version)
    log.Println(es.Info())
}
```

## Python

```
from datetime import datetime
from elasticsearch import Elasticsearch
es = Elasticsearch(['http://user:secret@xx.xx.xx.xx:9200'])

doc = {
    'author': 'kimchy',
    'text': 'Elasticsearch: cool. bonsai cool.',
    'timestamp': datetime.now(),
}
res = es.index(index="test-index", doc_type='tweet', id=1, body=doc)
print(res['result'])

res = es.get(index="test-index", doc_type='tweet', id=1)
print(res['_source'])

es.indices.refresh(index="test-index")

res = es.search(index="test-index", body={"query": {"match_all": {}}})
print("Got %d Hits:" % res['hits']['total'])
for hit in res['hits']['hits']:
    print("%(timestamp)s %(author)s: %(text)s" % hit["_source"])
```