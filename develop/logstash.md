# Logstash部署

## 安装Logstash

Logstash5.5需要Java 8，不支持Java 9。 使用官方的Oracle发行版或OpenJDK等开源发行版。

安装参考
\_**\[Installing-Logstash\](<https://www.elastic.co/guide/en/logstash/current/installing-logstash.html>)**\_

## 配置文件编写

要配置Logstash，需要创建一个配置文件，指定要使用的插件以及每个插件的设置。您可以引用配置中的事件字段，并使用条件来处理符合特定条件的事件。当运行logstash时，使用-f来指定配置文件。

创建一个名为 \`logstash-simple.conf\` 的文件，并将其保存在与Logstash相同的目录中。

``` conf
input { stdin { } }
output {
  elasticsearch { hosts => ["<host>:9200"] }
  stdout { codec => rubydebug }
}
```

运行logstash并使用-f标志指定配置文件。

```
bin/logstash -f logstash-simple.conf
```

更多配置示例参考
\_**\[Logstash-Config-Examples\](<https://www.elastic.co/guide/en/logstash/current/config-examples.html>)**\_