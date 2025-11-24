# 预置插件

UES 提供了插件机制，当前我们的 UES 服务内置了以下常用插件，暂不支持用户自己上传安装插件。

_[参考链接](https://www.elastic.co/guide/en/elasticsearch/plugins/current/index.html)_

## Analysis Plugins

  - **analysis-ik**

IK为常用的中文分词插件。需要注意的是，IK插件提供的中文分词仅对新生成并指定IK作为分词器的index有效，对之前创建的index无效。

  - **analysis-pinyin**

拼音分析插件，用来做汉字和拼音之间的转换。

  - **analysis-icu**

使用 ICU 实现的一个针对亚洲语言的分词器插件。

  - **analysis-kuromoji**

使用 Kuromoji analyzer 实现的一个日语的分词器插件。

  - **analysis-smartcn**

针对中文或者中英文混合的文本的分词器插件。

  - **analysis-ukrainian**

Provides stemming for Ukrainian.

## Ingest Plugins

  - **ingest-attachment**

使用 Apache Tika 实现的用来解析 PPT、XLS、PDF 和 Microsoft Word 等文档的插件。

  - **ingest-geoip**

GeoIP处理器根据Maxmind数据库的数据添加有关IP地址位置的信息。该处理器默认在地理位置字段下添加此信息。地理位置处理器可以解析IPv4和IPv6地址。

  - **ingest-user-agent**

该插件默认使用由uap-java提供的具有Apache 2.0许可证的regexes.yaml.
插件处理器从浏览器发送的用户代理字符串中提取其Web请求的详细信息。该处理器默认在user\_agent字段下添加此信息。

## Elasticsearch Head

elasticsearch-head是比较常用的集群管理工具。

官方ES在5.0以上版本中已不再提供直接安装head插件。内置的head插件做为一个独立的工程运行，head插件默认端口为9100。

## SQL

可以使用户以类SQL语法查询、分析存储在Elasticsearch中的数据。

**Basic Usage**

\* Simple query

```
/_sql?sql=select * from indexName limit 10
```

\* Explain SQL to elasticsearch query DSL

```
/_sql/_explain?sql=select * from indexName limit 10
```

**SQL Usage**

\* Query

```
SELECT * FROM bank WHERE age > 30 AND gender = 'm'
```

\* Aggregation

```
select COUNT(*),SUM(age),MIN(age) as m, MAX(age),AVG(age) FROM bank GROUP BY gender ORDER BY SUM(age), m DESC
```

\* Delete

```
DELETE FROM bank WHERE age > 30 AND gender = 'm'
```

**Beyond sql**

\* Search

```
SELECT address FROM bank WHERE address = matchQuery('880 Holmes Lane') ORDER BY _score DESC LIMIT 3
```

\* Aggregations

```
# range age group 20-25,25-30,30-35,35-40
SELECT COUNT(age) FROM bank GROUP BY range(age, 20,25,30,35,40)

# range date group by day
SELECT online FROM online GROUP BY date_histogram(field='insert_time','interval'='1d')

# range date group by your config
SELECT online FROM online GROUP BY date_range(field='insert_time','format'='yyyy-MM-dd' ,'2014-08-18','2014-08-17','now-8d','now-7d','now-6d','now')
```

\* ES Geographic

```
SELECT * FROM locations WHERE GEO_BOUNDING_BOX(fieldname,100.0,1.0,101,0.0)
```

\* Select type

```
SELECT * FROM indexName/type
```

**该插件使用来自开源项目**，_[参考链接](https://github.com/NLPchina/elasticsearch-sql)_

## Snapshot/Restore Repository Plugins

  - **Hadoop HDFS Repository Plugin**

该插件是针对最新的Apache Hadoop 2.x构建的，为使用HDFS文件系统作为快照/恢复存储库提供支持。