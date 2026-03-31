# OpenSearch 性能能测试

使用ESRally工具对数据节点规格为4核16 GiB的UCloud云 OpenSearch 3.3.2 版本的实例进行基准测试。

## **压测配置**

| **项目** | **说明**                                                                                                                                                                |
|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 实例配置 | - 版本：**3.3.2版本**<br> - 数据节点规格：**4核16 GiB**<br> - 数据节点存储类型：RSSD云盘<br> - 数据单节点存储空间：200 GiB<br> - 数据节点数量：3个                                                              |
| ESRally配置 | 使用ESRally的默认配置tracks。                                                                                                                                                 |
| 数据集 | 使用ESRally的预置数据集`geonames`模拟测试场景，数据集中的文档总数为11396503 。 获取数据集，请参见[通过Elasticsearch Rally Hub获取](https://github.com/elastic/rally-tracks/blob/master/http_logs/README.md)。 |
| 分片数 | - 主分片：6 - 副本分片：0                                                                                                                                                      |
| bulk_size | 每次批量操作提交2000个文档。                                                                                                                                                      |
| bulk_indexing_clients | 同时执行批量索引操作的客户端数量为6个。                                                                                                                                                  |

## 压测结果

压测结果仅供参考，无法代表实际生产中写入查询情况，建议您结合业务生产数据进行压测。

### **基准压测报告**

|                                                         Metric |                           Task |        Value |    Unit |
|---------------------------------------------------------------:|-------------------------------:|-------------:|--------:|
|                     Cumulative indexing time of primary shards |                                |  11.8876     |     min |
|             Min cumulative indexing time across primary shards |                                |   0          |     min |
|          Median cumulative indexing time across primary shards |                                |   0.002      |     min |
|             Max cumulative indexing time across primary shards |                                |   2.06043    |     min |
|            Cumulative indexing throttle time of primary shards |                                |   0          |     min |
|    Min cumulative indexing throttle time across primary shards |                                |   0          |     min |
| Median cumulative indexing throttle time across primary shards |                                |   0          |     min |
|    Max cumulative indexing throttle time across primary shards |                                |   0          |     min |
|                        Cumulative merge time of primary shards |                                |   1.4614     |     min |
|                       Cumulative merge count of primary shards |                                |  90          |         |
|                Min cumulative merge time across primary shards |                                |   0          |     min |
|             Median cumulative merge time across primary shards |                                |   0.0009     |     min |
|                Max cumulative merge time across primary shards |                                |   0.294483   |     min |
|               Cumulative merge throttle time of primary shards |                                |   0.418283   |     min |
|       Min cumulative merge throttle time across primary shards |                                |   0          |     min |
|    Median cumulative merge throttle time across primary shards |                                |   0          |     min |
|       Max cumulative merge throttle time across primary shards |                                |   0.123267   |     min |
|                      Cumulative refresh time of primary shards |                                |   1.93965    |     min |
|                     Cumulative refresh count of primary shards |                                | 980          |         |
|              Min cumulative refresh time across primary shards |                                |   0          |     min |
|           Median cumulative refresh time across primary shards |                                |   0.00143333 |     min |
|              Max cumulative refresh time across primary shards |                                |   0.36315    |     min |
|                        Cumulative flush time of primary shards |                                |   0.944667   |     min |
|                       Cumulative flush count of primary shards |                                |  26          |         |
|                Min cumulative flush time across primary shards |                                |   0          |     min |
|             Median cumulative flush time across primary shards |                                |   0          |     min |
|                Max cumulative flush time across primary shards |                                |   0.235317   |     min |
|                                        Total Young Gen GC time |                                |   4.054      |       s |
|                                       Total Young Gen GC count |                                | 217          |         |
|                                          Total Old Gen GC time |                                |   0          |       s |
|                                         Total Old Gen GC count |                                |   0          |         |
|                                                     Store size |                                |   6.18587    |      GB |
|                                                  Translog size |                                |   0.688198   |      GB |
|                                         Heap used for segments |                                |   0          |      MB |
|                                       Heap used for doc values |                                |   0          |      MB |
|                                            Heap used for terms |                                |   0          |      MB |
|                                            Heap used for norms |                                |   0          |      MB |
|                                           Heap used for points |                                |   0          |      MB |
|                                    Heap used for stored fields |                                |   0          |      MB |
|                                                  Segment count |                                |  85          |         |
|                                    Total Ingest Pipeline count |                                |   0          |         |
|                                     Total Ingest Pipeline time |                                |   0          |       s |
|                                   Total Ingest Pipeline failed |                                |   0          |         |
|                                                     error rate |                   index-append |   0          |       % |
|                                                 Min Throughput |                    index-stats |  89.69       |   ops/s |
|                                                Mean Throughput |                    index-stats |  89.82       |   ops/s |
|                                              Median Throughput |                    index-stats |  89.84       |   ops/s |
|                                                 Max Throughput |                    index-stats |  89.88       |   ops/s |
|                                        50th percentile latency |                    index-stats |   6.08095    |      ms |
|                                        90th percentile latency |                    index-stats |   7.30882    |      ms |
|                                        99th percentile latency |                    index-stats |  10.0216     |      ms |
|                                      99.9th percentile latency |                    index-stats |  19.7427     |      ms |
|                                       100th percentile latency |                    index-stats |  24.0911     |      ms |
|                                   50th percentile service time |                    index-stats |   5.19725    |      ms |
|                                   90th percentile service time |                    index-stats |   6.325      |      ms |
|                                   99th percentile service time |                    index-stats |   7.3915     |      ms |
|                                 99.9th percentile service time |                    index-stats |  10.9621     |      ms |
|                                  100th percentile service time |                    index-stats |  11.3456     |      ms |
|                                                     error rate |                    index-stats |   0          |       % |
|                                                 Min Throughput |                     node-stats |  88.68       |   ops/s |
|                                                Mean Throughput |                     node-stats |  89.5        |   ops/s |
|                                              Median Throughput |                     node-stats |  89.62       |   ops/s |
|                                                 Max Throughput |                     node-stats |  89.78       |   ops/s |
|                                        50th percentile latency |                     node-stats |   6.21964    |      ms |
|                                        90th percentile latency |                     node-stats |   7.18502    |      ms |
|                                        99th percentile latency |                     node-stats |  10.8955     |      ms |
|                                      99.9th percentile latency |                     node-stats |  18.9927     |      ms |
|                                       100th percentile latency |                     node-stats |  22.953      |      ms |
|                                   50th percentile service time |                     node-stats |   5.42969    |      ms |
|                                   90th percentile service time |                     node-stats |   6.29734    |      ms |
|                                   99th percentile service time |                     node-stats |   9.81883    |      ms |
|                                 99.9th percentile service time |                     node-stats |  12.6373     |      ms |
|                                  100th percentile service time |                     node-stats |  13.531      |      ms |
|                                                     error rate |                     node-stats |   0          |       % |
|                                                 Min Throughput |                        default |  49.86       |   ops/s |
|                                                Mean Throughput |                        default |  49.92       |   ops/s |
|                                              Median Throughput |                        default |  49.93       |   ops/s |
|                                                 Max Throughput |                        default |  49.95       |   ops/s |
|                                        50th percentile latency |                        default |   7.18032    |      ms |
|                                        90th percentile latency |                        default |   8.36214    |      ms |
|                                        99th percentile latency |                        default |   9.41984    |      ms |
|                                      99.9th percentile latency |                        default |  10.688      |      ms |
|                                       100th percentile latency |                        default |  13.3788     |      ms |
|                                   50th percentile service time |                        default |   5.90165    |      ms |
|                                   90th percentile service time |                        default |   6.77554    |      ms |
|                                   99th percentile service time |                        default |   7.74374    |      ms |
|                                 99.9th percentile service time |                        default |   9.29327    |      ms |
|                                  100th percentile service time |                        default |  12.725      |      ms |
|                                                     error rate |                        default |   0          |       % |
|                                                 Min Throughput |                           term |  99.87       |   ops/s |
|                                                Mean Throughput |                           term |  99.92       |   ops/s |
|                                              Median Throughput |                           term |  99.92       |   ops/s |
|                                                 Max Throughput |                           term |  99.95       |   ops/s |
|                                        50th percentile latency |                           term |   5.95651    |      ms |
|                                        90th percentile latency |                           term |   6.96999    |      ms |
|                                        99th percentile latency |                           term |   7.70642    |      ms |
|                                      99.9th percentile latency |                           term |   8.13199    |      ms |
|                                       100th percentile latency |                           term |   8.24946    |      ms |
|                                   50th percentile service time |                           term |   5.13072    |      ms |
|                                   90th percentile service time |                           term |   6.09026    |      ms |
|                                   99th percentile service time |                           term |   6.7958     |      ms |
|                                 99.9th percentile service time |                           term |   7.17034    |      ms |
|                                  100th percentile service time |                           term |   7.63978    |      ms |
|                                                     error rate |                           term |   0          |       % |
|                                                 Min Throughput |                         phrase | 108.92       |   ops/s |
|                                                Mean Throughput |                         phrase | 109.34       |   ops/s |
|                                              Median Throughput |                         phrase | 109.4        |   ops/s |
|                                                 Max Throughput |                         phrase | 109.58       |   ops/s |
|                                        50th percentile latency |                         phrase |   6.12094    |      ms |
|                                        90th percentile latency |                         phrase |   7.15696    |      ms |
|                                        99th percentile latency |                         phrase |   8.84119    |      ms |
|                                      99.9th percentile latency |                         phrase |  25.8928     |      ms |
|                                       100th percentile latency |                         phrase |  28.5679     |      ms |
|                                   50th percentile service time |                         phrase |   5.34935    |      ms |
|                                   90th percentile service time |                         phrase |   6.27261    |      ms |
|                                   99th percentile service time |                         phrase |   7.31547    |      ms |
|                                 99.9th percentile service time |                         phrase |   8.72698    |      ms |
|                                  100th percentile service time |                         phrase |  28.0649     |      ms |
|                                                     error rate |                         phrase |   0          |       % |
|                                                 Min Throughput |           country_agg_uncached |   2.99       |   ops/s |
|                                                Mean Throughput |           country_agg_uncached |   2.99       |   ops/s |
|                                              Median Throughput |           country_agg_uncached |   2.99       |   ops/s |
|                                                 Max Throughput |           country_agg_uncached |   2.99       |   ops/s |
|                                        50th percentile latency |           country_agg_uncached | 103.462      |      ms |
|                                        90th percentile latency |           country_agg_uncached | 119.365      |      ms |
|                                        99th percentile latency |           country_agg_uncached | 133.304      |      ms |
|                                       100th percentile latency |           country_agg_uncached | 133.787      |      ms |
|                                   50th percentile service time |           country_agg_uncached | 102.406      |      ms |
|                                   90th percentile service time |           country_agg_uncached | 117.784      |      ms |
|                                   99th percentile service time |           country_agg_uncached | 131.314      |      ms |
|                                  100th percentile service time |           country_agg_uncached | 131.93       |      ms |
|                                                     error rate |           country_agg_uncached |   0          |       % |
|                                                 Min Throughput |             country_agg_cached |  99.15       |   ops/s |
|                                                Mean Throughput |             country_agg_cached |  99.38       |   ops/s |
|                                              Median Throughput |             country_agg_cached |  99.4        |   ops/s |
|                                                 Max Throughput |             country_agg_cached |  99.53       |   ops/s |
|                                        50th percentile latency |             country_agg_cached |   4.99268    |      ms |
|                                        90th percentile latency |             country_agg_cached |   5.78334    |      ms |
|                                        99th percentile latency |             country_agg_cached |   6.53285    |      ms |
|                                      99.9th percentile latency |             country_agg_cached |   6.82266    |      ms |
|                                       100th percentile latency |             country_agg_cached |   6.82954    |      ms |
|                                   50th percentile service time |             country_agg_cached |   4.16732    |      ms |
|                                   90th percentile service time |             country_agg_cached |   4.86232    |      ms |
|                                   99th percentile service time |             country_agg_cached |   5.50475    |      ms |
|                                 99.9th percentile service time |             country_agg_cached |   6.05625    |      ms |
|                                  100th percentile service time |             country_agg_cached |   6.13466    |      ms |
|                                                     error rate |             country_agg_cached |   0          |       % |
|                                                 Min Throughput |                         scroll |  20.04       | pages/s |
|                                                Mean Throughput |                         scroll |  20.05       | pages/s |
|                                              Median Throughput |                         scroll |  20.05       | pages/s |
|                                                 Max Throughput |                         scroll |  20.06       | pages/s |
|                                        50th percentile latency |                         scroll | 234.244      |      ms |
|                                        90th percentile latency |                         scroll | 255.316      |      ms |
|                                        99th percentile latency |                         scroll | 274.374      |      ms |
|                                       100th percentile latency |                         scroll | 282.74       |      ms |
|                                   50th percentile service time |                         scroll | 232.279      |      ms |
|                                   90th percentile service time |                         scroll | 252.298      |      ms |
|                                   99th percentile service time |                         scroll | 270.798      |      ms |
|                                  100th percentile service time |                         scroll | 279.456      |      ms |
|                                                     error rate |                         scroll |   0          |       % |
|                                                 Min Throughput |                     expression |   1.5        |   ops/s |
|                                                Mean Throughput |                     expression |   1.5        |   ops/s |
|                                              Median Throughput |                     expression |   1.5        |   ops/s |
|                                                 Max Throughput |                     expression |   1.5        |   ops/s |
|                                        50th percentile latency |                     expression | 230.765      |      ms |
|                                        90th percentile latency |                     expression | 284.543      |      ms |
|                                        99th percentile latency |                     expression | 322.197      |      ms |
|                                       100th percentile latency |                     expression | 323.126      |      ms |
|                                   50th percentile service time |                     expression | 229.366      |      ms |
|                                   90th percentile service time |                     expression | 283.037      |      ms |
|                                   99th percentile service time |                     expression | 321.183      |      ms |
|                                  100th percentile service time |                     expression | 321.637      |      ms |
|                                                     error rate |                     expression |   0          |       % |
|                                                 Min Throughput |                painless_static |   1.4        |   ops/s |
|                                                Mean Throughput |                painless_static |   1.4        |   ops/s |
|                                              Median Throughput |                painless_static |   1.4        |   ops/s |
|                                                 Max Throughput |                painless_static |   1.4        |   ops/s |
|                                        50th percentile latency |                painless_static | 293.377      |      ms |
|                                        90th percentile latency |                painless_static | 340.271      |      ms |
|                                        99th percentile latency |                painless_static | 375.916      |      ms |
|                                       100th percentile latency |                painless_static | 389.078      |      ms |
|                                   50th percentile service time |                painless_static | 291.766      |      ms |
|                                   90th percentile service time |                painless_static | 339.204      |      ms |
|                                   99th percentile service time |                painless_static | 374.347      |      ms |
|                                  100th percentile service time |                painless_static | 388.405      |      ms |
|                                                     error rate |                painless_static |   0          |       % |
|                                                 Min Throughput |               painless_dynamic |   1.4        |   ops/s |
|                                                Mean Throughput |               painless_dynamic |   1.4        |   ops/s |
|                                              Median Throughput |               painless_dynamic |   1.4        |   ops/s |
|                                                 Max Throughput |               painless_dynamic |   1.4        |   ops/s |
|                                        50th percentile latency |               painless_dynamic | 303.643      |      ms |
|                                        90th percentile latency |               painless_dynamic | 353.645      |      ms |
|                                        99th percentile latency |               painless_dynamic | 375.746      |      ms |
|                                       100th percentile latency |               painless_dynamic | 382.455      |      ms |
|                                   50th percentile service time |               painless_dynamic | 302.589      |      ms |
|                                   90th percentile service time |               painless_dynamic | 351.172      |      ms |
|                                   99th percentile service time |               painless_dynamic | 374.027      |      ms |
|                                  100th percentile service time |               painless_dynamic | 381.661      |      ms |
|                                                     error rate |               painless_dynamic |   0          |       % |
|                                                 Min Throughput | decay_geo_gauss_function_score |   1          |   ops/s |
|                                                Mean Throughput | decay_geo_gauss_function_score |   1          |   ops/s |
|                                              Median Throughput | decay_geo_gauss_function_score |   1          |   ops/s |
|                                                 Max Throughput | decay_geo_gauss_function_score |   1          |   ops/s |
|                                        50th percentile latency | decay_geo_gauss_function_score | 304.761      |      ms |
|                                        90th percentile latency | decay_geo_gauss_function_score | 340.136      |      ms |
|                                        99th percentile latency | decay_geo_gauss_function_score | 378.248      |      ms |
|                                       100th percentile latency | decay_geo_gauss_function_score | 379.872      |      ms |
|                                   50th percentile service time | decay_geo_gauss_function_score | 302.581      |      ms |
|                                   90th percentile service time | decay_geo_gauss_function_score | 338.153      |      ms |
|                                   99th percentile service time | decay_geo_gauss_function_score | 376.082      |      ms |
|                                  100th percentile service time | decay_geo_gauss_function_score | 377.833      |      ms |
|                                                     error rate | decay_geo_gauss_function_score |   0          |       % |
|                                                 Min Throughput |   decay_geo_gauss_script_score |   1          |   ops/s |
|                                                Mean Throughput |   decay_geo_gauss_script_score |   1          |   ops/s |
|                                              Median Throughput |   decay_geo_gauss_script_score |   1          |   ops/s |
|                                                 Max Throughput |   decay_geo_gauss_script_score |   1          |   ops/s |
|                                        50th percentile latency |   decay_geo_gauss_script_score | 317.709      |      ms |
|                                        90th percentile latency |   decay_geo_gauss_script_score | 353.352      |      ms |
|                                        99th percentile latency |   decay_geo_gauss_script_score | 397.286      |      ms |
|                                       100th percentile latency |   decay_geo_gauss_script_score | 661.982      |      ms |
|                                   50th percentile service time |   decay_geo_gauss_script_score | 315.443      |      ms |
|                                   90th percentile service time |   decay_geo_gauss_script_score | 351.823      |      ms |
|                                   99th percentile service time |   decay_geo_gauss_script_score | 395.131      |      ms |
|                                  100th percentile service time |   decay_geo_gauss_script_score | 660.61       |      ms |
|                                                     error rate |   decay_geo_gauss_script_score |   0          |       % |
|                                                 Min Throughput |     field_value_function_score |   1.5        |   ops/s |
|                                                Mean Throughput |     field_value_function_score |   1.5        |   ops/s |
|                                              Median Throughput |     field_value_function_score |   1.5        |   ops/s |
|                                                 Max Throughput |     field_value_function_score |   1.5        |   ops/s |
|                                        50th percentile latency |     field_value_function_score | 115.782      |      ms |
|                                        90th percentile latency |     field_value_function_score | 136.708      |      ms |
|                                        99th percentile latency |     field_value_function_score | 155.311      |      ms |
|                                       100th percentile latency |     field_value_function_score | 393.029      |      ms |
|                                   50th percentile service time |     field_value_function_score | 114.51       |      ms |
|                                   90th percentile service time |     field_value_function_score | 135.332      |      ms |
|                                   99th percentile service time |     field_value_function_score | 153.492      |      ms |
|                                  100th percentile service time |     field_value_function_score | 391.75       |      ms |
|                                                     error rate |     field_value_function_score |   0          |       % |
|                                                 Min Throughput |       field_value_script_score |   1.5        |   ops/s |
|                                                Mean Throughput |       field_value_script_score |   1.5        |   ops/s |
|                                              Median Throughput |       field_value_script_score |   1.5        |   ops/s |
|                                                 Max Throughput |       field_value_script_score |   1.5        |   ops/s |
|                                        50th percentile latency |       field_value_script_score | 154.995      |      ms |
|                                        90th percentile latency |       field_value_script_score | 180.372      |      ms |
|                                        99th percentile latency |       field_value_script_score | 192.761      |      ms |
|                                       100th percentile latency |       field_value_script_score | 212.412      |      ms |
|                                   50th percentile service time |       field_value_script_score | 152.902      |      ms |
|                                   90th percentile service time |       field_value_script_score | 178.777      |      ms |
|                                   99th percentile service time |       field_value_script_score | 190.614      |      ms |
|                                  100th percentile service time |       field_value_script_score | 211.028      |      ms |
|                                                     error rate |       field_value_script_score |   0          |       % |
|                                                 Min Throughput |                    large_terms |   1.1        |   ops/s |
|                                                Mean Throughput |                    large_terms |   1.1        |   ops/s |
|                                              Median Throughput |                    large_terms |   1.1        |   ops/s |
|                                                 Max Throughput |                    large_terms |   1.1        |   ops/s |
|                                        50th percentile latency |                    large_terms | 230.827      |      ms |
|                                        90th percentile latency |                    large_terms | 262.865      |      ms |
|                                        99th percentile latency |                    large_terms | 283.087      |      ms |
|                                       100th percentile latency |                    large_terms | 312.985      |      ms |
|                                   50th percentile service time |                    large_terms | 219.731      |      ms |
|                                   90th percentile service time |                    large_terms | 251.838      |      ms |
|                                   99th percentile service time |                    large_terms | 271.845      |      ms |
|                                  100th percentile service time |                    large_terms | 301.738      |      ms |
|                                                     error rate |                    large_terms |   0          |       % |
|                                                 Min Throughput |           large_filtered_terms |   1.1        |   ops/s |
|                                                Mean Throughput |           large_filtered_terms |   1.1        |   ops/s |
|                                              Median Throughput |           large_filtered_terms |   1.1        |   ops/s |
|                                                 Max Throughput |           large_filtered_terms |   1.1        |   ops/s |
|                                        50th percentile latency |           large_filtered_terms | 246.334      |      ms |
|                                        90th percentile latency |           large_filtered_terms | 274.307      |      ms |
|                                        99th percentile latency |           large_filtered_terms | 305.979      |      ms |
|                                       100th percentile latency |           large_filtered_terms | 328.524      |      ms |
|                                   50th percentile service time |           large_filtered_terms | 235.227      |      ms |
|                                   90th percentile service time |           large_filtered_terms | 263.367      |      ms |
|                                   99th percentile service time |           large_filtered_terms | 295.324      |      ms |
|                                  100th percentile service time |           large_filtered_terms | 317.711      |      ms |
|                                                     error rate |           large_filtered_terms |   0          |       % |
|                                                 Min Throughput |         large_prohibited_terms |   1.1        |   ops/s |
|                                                Mean Throughput |         large_prohibited_terms |   1.1        |   ops/s |
|                                              Median Throughput |         large_prohibited_terms |   1.1        |   ops/s |
|                                                 Max Throughput |         large_prohibited_terms |   1.1        |   ops/s |
|                                        50th percentile latency |         large_prohibited_terms | 242.831      |      ms |
|                                        90th percentile latency |         large_prohibited_terms | 265.959      |      ms |
|                                        99th percentile latency |         large_prohibited_terms | 294.733      |      ms |
|                                       100th percentile latency |         large_prohibited_terms | 301.59       |      ms |
|                                   50th percentile service time |         large_prohibited_terms | 232.986      |      ms |
|                                   90th percentile service time |         large_prohibited_terms | 254.651      |      ms |
|                                   99th percentile service time |         large_prohibited_terms | 285.118      |      ms |
|                                  100th percentile service time |         large_prohibited_terms | 292.054      |      ms |
|                                                     error rate |         large_prohibited_terms |   0          |       % |
|                                                 Min Throughput |           desc_sort_population |   1.5        |   ops/s |
|                                                Mean Throughput |           desc_sort_population |   1.51       |   ops/s |
|                                              Median Throughput |           desc_sort_population |   1.51       |   ops/s |
|                                                 Max Throughput |           desc_sort_population |   1.51       |   ops/s |
|                                        50th percentile latency |           desc_sort_population |  10.7143     |      ms |
|                                        90th percentile latency |           desc_sort_population |  12.4974     |      ms |
|                                        99th percentile latency |           desc_sort_population |  16.9529     |      ms |
|                                       100th percentile latency |           desc_sort_population |  17.1341     |      ms |
|                                   50th percentile service time |           desc_sort_population |   9.10793    |      ms |
|                                   90th percentile service time |           desc_sort_population |  11.1575     |      ms |
|                                   99th percentile service time |           desc_sort_population |  15.6916     |      ms |
|                                  100th percentile service time |           desc_sort_population |  15.8726     |      ms |
|                                                     error rate |           desc_sort_population |   0          |       % |
|                                                 Min Throughput |            asc_sort_population |   1.5        |   ops/s |
|                                                Mean Throughput |            asc_sort_population |   1.51       |   ops/s |
|                                              Median Throughput |            asc_sort_population |   1.51       |   ops/s |
|                                                 Max Throughput |            asc_sort_population |   1.51       |   ops/s |
|                                        50th percentile latency |            asc_sort_population |   9.06536    |      ms |
|                                        90th percentile latency |            asc_sort_population |  10.2045     |      ms |
|                                        99th percentile latency |            asc_sort_population |  11.4211     |      ms |
|                                       100th percentile latency |            asc_sort_population |  12.5069     |      ms |
|                                   50th percentile service time |            asc_sort_population |   7.50035    |      ms |
|                                   90th percentile service time |            asc_sort_population |   8.54253    |      ms |
|                                   99th percentile service time |            asc_sort_population |   9.93852    |      ms |
|                                  100th percentile service time |            asc_sort_population |  10.8893     |      ms |
|                                                     error rate |            asc_sort_population |   0          |       % |
|                                                 Min Throughput | asc_sort_with_after_population |   1.5        |   ops/s |
|                                                Mean Throughput | asc_sort_with_after_population |   1.51       |   ops/s |
|                                              Median Throughput | asc_sort_with_after_population |   1.51       |   ops/s |
|                                                 Max Throughput | asc_sort_with_after_population |   1.51       |   ops/s |
|                                        50th percentile latency | asc_sort_with_after_population |   9.40269    |      ms |
|                                        90th percentile latency | asc_sort_with_after_population |  10.8795     |      ms |
|                                        99th percentile latency | asc_sort_with_after_population |  11.6739     |      ms |
|                                       100th percentile latency | asc_sort_with_after_population |  11.839      |      ms |
|                                   50th percentile service time | asc_sort_with_after_population |   7.92378    |      ms |
|                                   90th percentile service time | asc_sort_with_after_population |   9.17149    |      ms |
|                                   99th percentile service time | asc_sort_with_after_population |   9.99085    |      ms |
|                                  100th percentile service time | asc_sort_with_after_population |  10.1927     |      ms |
|                                                     error rate | asc_sort_with_after_population |   0          |       % |
|                                                 Min Throughput |            desc_sort_geonameid |   6.02       |   ops/s |
|                                                Mean Throughput |            desc_sort_geonameid |   6.02       |   ops/s |
|                                              Median Throughput |            desc_sort_geonameid |   6.02       |   ops/s |
|                                                 Max Throughput |            desc_sort_geonameid |   6.03       |   ops/s |
|                                        50th percentile latency |            desc_sort_geonameid |  11.104      |      ms |
|                                        90th percentile latency |            desc_sort_geonameid |  12.7472     |      ms |
|                                        99th percentile latency |            desc_sort_geonameid |  13.6321     |      ms |
|                                       100th percentile latency |            desc_sort_geonameid |  14.0339     |      ms |
|                                   50th percentile service time |            desc_sort_geonameid |   9.94169    |      ms |
|                                   90th percentile service time |            desc_sort_geonameid |  11.9943     |      ms |
|                                   99th percentile service time |            desc_sort_geonameid |  12.8391     |      ms |
|                                  100th percentile service time |            desc_sort_geonameid |  13.152      |      ms |
|                                                     error rate |            desc_sort_geonameid |   0          |       % |
|                                                 Min Throughput | desc_sort_with_after_geonameid |   6.02       |   ops/s |
|                                                Mean Throughput | desc_sort_with_after_geonameid |   6.02       |   ops/s |
|                                              Median Throughput | desc_sort_with_after_geonameid |   6.02       |   ops/s |
|                                                 Max Throughput | desc_sort_with_after_geonameid |   6.03       |   ops/s |
|                                        50th percentile latency | desc_sort_with_after_geonameid |   9.44969    |      ms |
|                                        90th percentile latency | desc_sort_with_after_geonameid |  11.194      |      ms |
|                                        99th percentile latency | desc_sort_with_after_geonameid |  11.9378     |      ms |
|                                       100th percentile latency | desc_sort_with_after_geonameid |  12.3591     |      ms |
|                                   50th percentile service time | desc_sort_with_after_geonameid |   8.47208    |      ms |
|                                   90th percentile service time | desc_sort_with_after_geonameid |   9.83766    |      ms |
|                                   99th percentile service time | desc_sort_with_after_geonameid |  10.9564     |      ms |
|                                  100th percentile service time | desc_sort_with_after_geonameid |  11.0534     |      ms |
|                                                     error rate | desc_sort_with_after_geonameid |   0          |       % |
|                                                 Min Throughput |             asc_sort_geonameid |   6.02       |   ops/s |
|                                                Mean Throughput |             asc_sort_geonameid |   6.02       |   ops/s |
|                                              Median Throughput |             asc_sort_geonameid |   6.02       |   ops/s |
|                                                 Max Throughput |             asc_sort_geonameid |   6.03       |   ops/s |
|                                        50th percentile latency |             asc_sort_geonameid |   8.55918    |      ms |
|                                        90th percentile latency |             asc_sort_geonameid |   9.46954    |      ms |
|                                        99th percentile latency |             asc_sort_geonameid |   9.9893     |      ms |
|                                       100th percentile latency |             asc_sort_geonameid |  10.2508     |      ms |
|                                   50th percentile service time |             asc_sort_geonameid |   7.27712    |      ms |
|                                   90th percentile service time |             asc_sort_geonameid |   8.10229    |      ms |
|                                   99th percentile service time |             asc_sort_geonameid |   8.91402    |      ms |
|                                  100th percentile service time |             asc_sort_geonameid |   9.24988    |      ms |
|                                                     error rate |             asc_sort_geonameid |   0          |       % |
|                                                 Min Throughput |  asc_sort_with_after_geonameid |   6.02       |   ops/s |
|                                                Mean Throughput |  asc_sort_with_after_geonameid |   6.02       |   ops/s |
|                                              Median Throughput |  asc_sort_with_after_geonameid |   6.02       |   ops/s |
|                                                 Max Throughput |  asc_sort_with_after_geonameid |   6.03       |   ops/s |
|                                        50th percentile latency |  asc_sort_with_after_geonameid |   9.32344    |      ms |
|                                        90th percentile latency |  asc_sort_with_after_geonameid |  10.7245     |      ms |
|                                        99th percentile latency |  asc_sort_with_after_geonameid |  12.0287     |      ms |
|                                       100th percentile latency |  asc_sort_with_after_geonameid |  12.1895     |      ms |
|                                   50th percentile service time |  asc_sort_with_after_geonameid |   8.02814    |      ms |
|                                   90th percentile service time |  asc_sort_with_after_geonameid |   9.38095    |      ms |
|                                   99th percentile service time |  asc_sort_with_after_geonameid |  10.6442     |      ms |
|                                  100th percentile service time |  asc_sort_with_after_geonameid |  11.0469     |      ms |
|                                                     error rate |  asc_sort_with_after_geonameid |   0          |       % |
