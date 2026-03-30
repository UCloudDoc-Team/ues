# UES 性能能测试

使用ESRally工具对数据节点规格为4核16 GiB的UCloud云 Elasticsearch OSS 7.10.2 版本的实例进行基准测试。

## **压测配置**

| **项目** | **说明**                                                                                                                                                          |
|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 实例配置 | - 版本：** OSS 7.10.2版本**  - 数据节点规格：**4核16 GiB** - 数据节点存储类型：RSSD云盘 - 数据单节点存储空间：200 GiB - 数据节点数量：3个                                                                 |
| ESRally配置 | 使用ESRally的默认配置tracks。                                                                                                                                           |
| 数据集 | 使用ESRally的预置数据集`geonames`模拟测试场景，数据集中的文档总数为11396503 。 获取数据集，请参见[通过Elasticsearch Rally Hub获取](https://github.com/elastic/rally-tracks/blob/master/http_logs/README.md)。 |
| 分片数 | - 主分片：6 - 副本分片：0                                                                                                                                                |
| bulk_size | 每次批量操作提交2000个文档。                                                                                                                                                |
| bulk_indexing_clients | 同时执行批量索引操作的客户端数量为6个。                                                                                                                                            |

## 压测结果

压测结果仅供参考，无法代表实际生产中写入查询情况，建议您结合业务生产数据进行压测。

### **基准压测报告**

|                                                         Metric |                           Task |         Value |    Unit |
|---------------------------------------------------------------:|-------------------------------:|--------------:|--------:|
|                     Cumulative indexing time of primary shards |                                |  10.8462      |     min |
|             Min cumulative indexing time across primary shards |                                |   0.00025     |     min |
|          Median cumulative indexing time across primary shards |                                |   1.74348     |     min |
|             Max cumulative indexing time across primary shards |                                |   2.09263     |     min |
|            Cumulative indexing throttle time of primary shards |                                |   0           |     min |
|    Min cumulative indexing throttle time across primary shards |                                |   0           |     min |
| Median cumulative indexing throttle time across primary shards |                                |   0           |     min |
|    Max cumulative indexing throttle time across primary shards |                                |   0           |     min |
|                        Cumulative merge time of primary shards |                                |   0.438667    |     min |
|                       Cumulative merge count of primary shards |                                |  11           |         |
|                Min cumulative merge time across primary shards |                                |   0           |     min |
|             Median cumulative merge time across primary shards |                                |   0.05645     |     min |
|                Max cumulative merge time across primary shards |                                |   0.126683    |     min |
|               Cumulative merge throttle time of primary shards |                                |   0           |     min |
|       Min cumulative merge throttle time across primary shards |                                |   0           |     min |
|    Median cumulative merge throttle time across primary shards |                                |   0           |     min |
|       Max cumulative merge throttle time across primary shards |                                |   0           |     min |
|                      Cumulative refresh time of primary shards |                                |   2.0197      |     min |
|                     Cumulative refresh count of primary shards |                                | 117           |         |
|              Min cumulative refresh time across primary shards |                                |   0.0005      |     min |
|           Median cumulative refresh time across primary shards |                                |   0.3131      |     min |
|              Max cumulative refresh time across primary shards |                                |   0.406883    |     min |
|                        Cumulative flush time of primary shards |                                |   0.326417    |     min |
|                       Cumulative flush count of primary shards |                                |  15           |         |
|                Min cumulative flush time across primary shards |                                |   0.000216667 |     min |
|             Median cumulative flush time across primary shards |                                |   0.0319667   |     min |
|                Max cumulative flush time across primary shards |                                |   0.0923333   |     min |
|                                        Total Young Gen GC time |                                |   3.736       |       s |
|                                       Total Young Gen GC count |                                | 246           |         |
|                                          Total Old Gen GC time |                                |   0           |       s |
|                                         Total Old Gen GC count |                                |   0           |         |
|                                                     Store size |                                |   2.95117     |      GB |
|                                                  Translog size |                                |   4.09782e-07 |      GB |
|                                         Heap used for segments |                                |   0.811859    |      MB |
|                                       Heap used for doc values |                                |   0.0477753   |      MB |
|                                            Heap used for terms |                                |   0.624207    |      MB |
|                                            Heap used for norms |                                |   0.0846558   |      MB |
|                                           Heap used for points |                                |   0           |      MB |
|                                    Heap used for stored fields |                                |   0.0552216   |      MB |
|                                                  Segment count |                                | 112           |         |
|                                    Total Ingest Pipeline count |                                |   0           |         |
|                                     Total Ingest Pipeline time |                                |   0           |       s |
|                                   Total Ingest Pipeline failed |                                |   0           |         |
|                                                     error rate |                   index-append |   0           |       % |
|                                                 Min Throughput |                    index-stats |  90           |   ops/s |
|                                                Mean Throughput |                    index-stats |  90.01        |   ops/s |
|                                              Median Throughput |                    index-stats |  90.01        |   ops/s |
|                                                 Max Throughput |                    index-stats |  90.02        |   ops/s |
|                                        50th percentile latency |                    index-stats |   3.93522     |      ms |
|                                        90th percentile latency |                    index-stats |   4.61968     |      ms |
|                                        99th percentile latency |                    index-stats |   5.54248     |      ms |
|                                      99.9th percentile latency |                    index-stats |   7.5565      |      ms |
|                                       100th percentile latency |                    index-stats |   9.18881     |      ms |
|                                   50th percentile service time |                    index-stats |   3.13227     |      ms |
|                                   90th percentile service time |                    index-stats |   3.6235      |      ms |
|                                   99th percentile service time |                    index-stats |   4.38468     |      ms |
|                                 99.9th percentile service time |                    index-stats |   6.90571     |      ms |
|                                  100th percentile service time |                    index-stats |   7.93799     |      ms |
|                                                     error rate |                    index-stats |   0           |       % |
|                                                 Min Throughput |                     node-stats |  89.82        |   ops/s |
|                                                Mean Throughput |                     node-stats |  89.94        |   ops/s |
|                                              Median Throughput |                     node-stats |  89.96        |   ops/s |
|                                                 Max Throughput |                     node-stats |  89.97        |   ops/s |
|                                        50th percentile latency |                     node-stats |   4.76822     |      ms |
|                                        90th percentile latency |                     node-stats |   5.66223     |      ms |
|                                        99th percentile latency |                     node-stats |   6.96857     |      ms |
|                                      99.9th percentile latency |                     node-stats |   8.39103     |      ms |
|                                       100th percentile latency |                     node-stats |   8.42407     |      ms |
|                                   50th percentile service time |                     node-stats |   3.97994     |      ms |
|                                   90th percentile service time |                     node-stats |   4.73088     |      ms |
|                                   99th percentile service time |                     node-stats |   5.96067     |      ms |
|                                 99.9th percentile service time |                     node-stats |   7.18339     |      ms |
|                                  100th percentile service time |                     node-stats |   7.37516     |      ms |
|                                                     error rate |                     node-stats |   0           |       % |
|                                                 Min Throughput |                        default |  49.78        |   ops/s |
|                                                Mean Throughput |                        default |  49.87        |   ops/s |
|                                              Median Throughput |                        default |  49.88        |   ops/s |
|                                                 Max Throughput |                        default |  49.92        |   ops/s |
|                                        50th percentile latency |                        default |   7.10286     |      ms |
|                                        90th percentile latency |                        default |   8.19486     |      ms |
|                                        99th percentile latency |                        default |   9.57382     |      ms |
|                                      99.9th percentile latency |                        default |  24.5467      |      ms |
|                                       100th percentile latency |                        default |  29.1593      |      ms |
|                                   50th percentile service time |                        default |   5.75317     |      ms |
|                                   90th percentile service time |                        default |   6.39686     |      ms |
|                                   99th percentile service time |                        default |   7.21485     |      ms |
|                                 99.9th percentile service time |                        default |  24.1797      |      ms |
|                                  100th percentile service time |                        default |  28.0118      |      ms |
|                                                     error rate |                        default |   0           |       % |
|                                                 Min Throughput |                           term |  99.75        |   ops/s |
|                                                Mean Throughput |                           term |  99.85        |   ops/s |
|                                              Median Throughput |                           term |  99.86        |   ops/s |
|                                                 Max Throughput |                           term |  99.9         |   ops/s |
|                                        50th percentile latency |                           term |   4.84599     |      ms |
|                                        90th percentile latency |                           term |   5.78255     |      ms |
|                                        99th percentile latency |                           term |  11.6061      |      ms |
|                                      99.9th percentile latency |                           term |  34.8432      |      ms |
|                                       100th percentile latency |                           term |  37.6201      |      ms |
|                                   50th percentile service time |                           term |   4.03951     |      ms |
|                                   90th percentile service time |                           term |   4.88257     |      ms |
|                                   99th percentile service time |                           term |   5.5992      |      ms |
|                                 99.9th percentile service time |                           term |  33.6985      |      ms |
|                                  100th percentile service time |                           term |  37.0368      |      ms |
|                                                     error rate |                           term |   0           |       % |
|                                                 Min Throughput |                         phrase | 109.45        |   ops/s |
|                                                Mean Throughput |                         phrase | 109.67        |   ops/s |
|                                              Median Throughput |                         phrase | 109.7         |   ops/s |
|                                                 Max Throughput |                         phrase | 109.79        |   ops/s |
|                                        50th percentile latency |                         phrase |   4.26436     |      ms |
|                                        90th percentile latency |                         phrase |   5.06721     |      ms |
|                                        99th percentile latency |                         phrase |   5.90272     |      ms |
|                                      99.9th percentile latency |                         phrase |   8.40009     |      ms |
|                                       100th percentile latency |                         phrase |   9.29318     |      ms |
|                                   50th percentile service time |                         phrase |   3.42861     |      ms |
|                                   90th percentile service time |                         phrase |   4.07208     |      ms |
|                                   99th percentile service time |                         phrase |   4.72354     |      ms |
|                                 99.9th percentile service time |                         phrase |   7.2153      |      ms |
|                                  100th percentile service time |                         phrase |   8.59297     |      ms |
|                                                     error rate |                         phrase |   0           |       % |
|                                                 Min Throughput |           country_agg_uncached |   3           |   ops/s |
|                                                Mean Throughput |           country_agg_uncached |   3           |   ops/s |
|                                              Median Throughput |           country_agg_uncached |   3           |   ops/s |
|                                                 Max Throughput |           country_agg_uncached |   3           |   ops/s |
|                                        50th percentile latency |           country_agg_uncached | 154.924       |      ms |
|                                        90th percentile latency |           country_agg_uncached | 181.559       |      ms |
|                                        99th percentile latency |           country_agg_uncached | 197.383       |      ms |
|                                       100th percentile latency |           country_agg_uncached | 201.002       |      ms |
|                                   50th percentile service time |           country_agg_uncached | 153.867       |      ms |
|                                   90th percentile service time |           country_agg_uncached | 180.312       |      ms |
|                                   99th percentile service time |           country_agg_uncached | 195.872       |      ms |
|                                  100th percentile service time |           country_agg_uncached | 199.396       |      ms |
|                                                     error rate |           country_agg_uncached |   0           |       % |
|                                                 Min Throughput |             country_agg_cached |  98.28        |   ops/s |
|                                                Mean Throughput |             country_agg_cached |  98.74        |   ops/s |
|                                              Median Throughput |             country_agg_cached |  98.78        |   ops/s |
|                                                 Max Throughput |             country_agg_cached |  99.06        |   ops/s |
|                                        50th percentile latency |             country_agg_cached |   3.50169     |      ms |
|                                        90th percentile latency |             country_agg_cached |   4.15876     |      ms |
|                                        99th percentile latency |             country_agg_cached |   4.91705     |      ms |
|                                      99.9th percentile latency |             country_agg_cached |   5.52199     |      ms |
|                                       100th percentile latency |             country_agg_cached |   5.76317     |      ms |
|                                   50th percentile service time |             country_agg_cached |   2.66331     |      ms |
|                                   90th percentile service time |             country_agg_cached |   3.23824     |      ms |
|                                   99th percentile service time |             country_agg_cached |   3.78035     |      ms |
|                                 99.9th percentile service time |             country_agg_cached |   4.21207     |      ms |
|                                  100th percentile service time |             country_agg_cached |   4.22923     |      ms |
|                                                     error rate |             country_agg_cached |   0           |       % |
|                                                 Min Throughput |                         scroll |  20.04        | pages/s |
|                                                Mean Throughput |                         scroll |  20.05        | pages/s |
|                                              Median Throughput |                         scroll |  20.04        | pages/s |
|                                                 Max Throughput |                         scroll |  20.06        | pages/s |
|                                        50th percentile latency |                         scroll | 339.382       |      ms |
|                                        90th percentile latency |                         scroll | 358.516       |      ms |
|                                        99th percentile latency |                         scroll | 373.911       |      ms |
|                                       100th percentile latency |                         scroll | 392.798       |      ms |
|                                   50th percentile service time |                         scroll | 337.296       |      ms |
|                                   90th percentile service time |                         scroll | 355.547       |      ms |
|                                   99th percentile service time |                         scroll | 371.94        |      ms |
|                                  100th percentile service time |                         scroll | 389.85        |      ms |
|                                                     error rate |                         scroll |   0           |       % |
|                                                 Min Throughput |                     expression |   1.5         |   ops/s |
|                                                Mean Throughput |                     expression |   1.5         |   ops/s |
|                                              Median Throughput |                     expression |   1.5         |   ops/s |
|                                                 Max Throughput |                     expression |   1.5         |   ops/s |
|                                        50th percentile latency |                     expression | 266.403       |      ms |
|                                        90th percentile latency |                     expression | 293.523       |      ms |
|                                        99th percentile latency |                     expression | 324.833       |      ms |
|                                       100th percentile latency |                     expression | 333.592       |      ms |
|                                   50th percentile service time |                     expression | 264.273       |      ms |
|                                   90th percentile service time |                     expression | 292.24        |      ms |
|                                   99th percentile service time |                     expression | 323.323       |      ms |
|                                  100th percentile service time |                     expression | 332.27        |      ms |
|                                                     error rate |                     expression |   0           |       % |
|                                                 Min Throughput |                painless_static |   1.4         |   ops/s |
|                                                Mean Throughput |                painless_static |   1.4         |   ops/s |
|                                              Median Throughput |                painless_static |   1.4         |   ops/s |
|                                                 Max Throughput |                painless_static |   1.4         |   ops/s |
|                                        50th percentile latency |                painless_static | 314.452       |      ms |
|                                        90th percentile latency |                painless_static | 357.594       |      ms |
|                                        99th percentile latency |                painless_static | 387.346       |      ms |
|                                       100th percentile latency |                painless_static | 410.92        |      ms |
|                                   50th percentile service time |                painless_static | 313.104       |      ms |
|                                   90th percentile service time |                painless_static | 356.293       |      ms |
|                                   99th percentile service time |                painless_static | 386.523       |      ms |
|                                  100th percentile service time |                painless_static | 409.092       |      ms |
|                                                     error rate |                painless_static |   0           |       % |
|                                                 Min Throughput |               painless_dynamic |   1.4         |   ops/s |
|                                                Mean Throughput |               painless_dynamic |   1.4         |   ops/s |
|                                              Median Throughput |               painless_dynamic |   1.4         |   ops/s |
|                                                 Max Throughput |               painless_dynamic |   1.4         |   ops/s |
|                                        50th percentile latency |               painless_dynamic | 310.825       |      ms |
|                                        90th percentile latency |               painless_dynamic | 345.03        |      ms |
|                                        99th percentile latency |               painless_dynamic | 376.961       |      ms |
|                                       100th percentile latency |               painless_dynamic | 382.331       |      ms |
|                                   50th percentile service time |               painless_dynamic | 309.708       |      ms |
|                                   90th percentile service time |               painless_dynamic | 343.595       |      ms |
|                                   99th percentile service time |               painless_dynamic | 375.491       |      ms |
|                                  100th percentile service time |               painless_dynamic | 380.625       |      ms |
|                                                     error rate |               painless_dynamic |   0           |       % |
|                                                 Min Throughput | decay_geo_gauss_function_score |   1           |   ops/s |
|                                                Mean Throughput | decay_geo_gauss_function_score |   1           |   ops/s |
|                                              Median Throughput | decay_geo_gauss_function_score |   1           |   ops/s |
|                                                 Max Throughput | decay_geo_gauss_function_score |   1           |   ops/s |
|                                        50th percentile latency | decay_geo_gauss_function_score | 321.308       |      ms |
|                                        90th percentile latency | decay_geo_gauss_function_score | 344.471       |      ms |
|                                        99th percentile latency | decay_geo_gauss_function_score | 385.476       |      ms |
|                                       100th percentile latency | decay_geo_gauss_function_score | 414.15        |      ms |
|                                   50th percentile service time | decay_geo_gauss_function_score | 319.176       |      ms |
|                                   90th percentile service time | decay_geo_gauss_function_score | 342.963       |      ms |
|                                   99th percentile service time | decay_geo_gauss_function_score | 383.724       |      ms |
|                                  100th percentile service time | decay_geo_gauss_function_score | 412.704       |      ms |
|                                                     error rate | decay_geo_gauss_function_score |   0           |       % |
|                                                 Min Throughput |   decay_geo_gauss_script_score |   1           |   ops/s |
|                                                Mean Throughput |   decay_geo_gauss_script_score |   1           |   ops/s |
|                                              Median Throughput |   decay_geo_gauss_script_score |   1           |   ops/s |
|                                                 Max Throughput |   decay_geo_gauss_script_score |   1           |   ops/s |
|                                        50th percentile latency |   decay_geo_gauss_script_score | 336.403       |      ms |
|                                        90th percentile latency |   decay_geo_gauss_script_score | 361.037       |      ms |
|                                        99th percentile latency |   decay_geo_gauss_script_score | 397.706       |      ms |
|                                       100th percentile latency |   decay_geo_gauss_script_score | 414.539       |      ms |
|                                   50th percentile service time |   decay_geo_gauss_script_score | 335.045       |      ms |
|                                   90th percentile service time |   decay_geo_gauss_script_score | 359.443       |      ms |
|                                   99th percentile service time |   decay_geo_gauss_script_score | 395.915       |      ms |
|                                  100th percentile service time |   decay_geo_gauss_script_score | 412.589       |      ms |
|                                                     error rate |   decay_geo_gauss_script_score |   0           |       % |
|                                                 Min Throughput |     field_value_function_score |   1.5         |   ops/s |
|                                                Mean Throughput |     field_value_function_score |   1.5         |   ops/s |
|                                              Median Throughput |     field_value_function_score |   1.5         |   ops/s |
|                                                 Max Throughput |     field_value_function_score |   1.51        |   ops/s |
|                                        50th percentile latency |     field_value_function_score | 125.031       |      ms |
|                                        90th percentile latency |     field_value_function_score | 138.476       |      ms |
|                                        99th percentile latency |     field_value_function_score | 148.138       |      ms |
|                                       100th percentile latency |     field_value_function_score | 155.558       |      ms |
|                                   50th percentile service time |     field_value_function_score | 123.228       |      ms |
|                                   90th percentile service time |     field_value_function_score | 136.664       |      ms |
|                                   99th percentile service time |     field_value_function_score | 146.235       |      ms |
|                                  100th percentile service time |     field_value_function_score | 152.966       |      ms |
|                                                     error rate |     field_value_function_score |   0           |       % |
|                                                 Min Throughput |       field_value_script_score |   1.5         |   ops/s |
|                                                Mean Throughput |       field_value_script_score |   1.5         |   ops/s |
|                                              Median Throughput |       field_value_script_score |   1.5         |   ops/s |
|                                                 Max Throughput |       field_value_script_score |   1.5         |   ops/s |
|                                        50th percentile latency |       field_value_script_score | 149.047       |      ms |
|                                        90th percentile latency |       field_value_script_score | 179.626       |      ms |
|                                        99th percentile latency |       field_value_script_score | 198.672       |      ms |
|                                       100th percentile latency |       field_value_script_score | 219.017       |      ms |
|                                   50th percentile service time |       field_value_script_score | 147.892       |      ms |
|                                   90th percentile service time |       field_value_script_score | 178.15        |      ms |
|                                   99th percentile service time |       field_value_script_score | 197.352       |      ms |
|                                  100th percentile service time |       field_value_script_score | 218.087       |      ms |
|                                                     error rate |       field_value_script_score |   0           |       % |
|                                                 Min Throughput |                    large_terms |   1.1         |   ops/s |
|                                                Mean Throughput |                    large_terms |   1.1         |   ops/s |
|                                              Median Throughput |                    large_terms |   1.1         |   ops/s |
|                                                 Max Throughput |                    large_terms |   1.1         |   ops/s |
|                                        50th percentile latency |                    large_terms | 687.623       |      ms |
|                                        90th percentile latency |                    large_terms | 729.572       |      ms |
|                                        99th percentile latency |                    large_terms | 813.962       |      ms |
|                                       100th percentile latency |                    large_terms | 830.134       |      ms |
|                                   50th percentile service time |                    large_terms | 673.161       |      ms |
|                                   90th percentile service time |                    large_terms | 715.132       |      ms |
|                                   99th percentile service time |                    large_terms | 800.056       |      ms |
|                                  100th percentile service time |                    large_terms | 817.885       |      ms |
|                                                     error rate |                    large_terms |   0           |       % |
|                                                 Min Throughput |           large_filtered_terms |   1.1         |   ops/s |
|                                                Mean Throughput |           large_filtered_terms |   1.1         |   ops/s |
|                                              Median Throughput |           large_filtered_terms |   1.1         |   ops/s |
|                                                 Max Throughput |           large_filtered_terms |   1.1         |   ops/s |
|                                        50th percentile latency |           large_filtered_terms | 673.398       |      ms |
|                                        90th percentile latency |           large_filtered_terms | 717.428       |      ms |
|                                        99th percentile latency |           large_filtered_terms | 752.508       |      ms |
|                                       100th percentile latency |           large_filtered_terms | 754.857       |      ms |
|                                   50th percentile service time |           large_filtered_terms | 661.094       |      ms |
|                                   90th percentile service time |           large_filtered_terms | 705.93        |      ms |
|                                   99th percentile service time |           large_filtered_terms | 741.282       |      ms |
|                                  100th percentile service time |           large_filtered_terms | 741.426       |      ms |
|                                                     error rate |           large_filtered_terms |   0           |       % |
|                                                 Min Throughput |         large_prohibited_terms |   1.1         |   ops/s |
|                                                Mean Throughput |         large_prohibited_terms |   1.1         |   ops/s |
|                                              Median Throughput |         large_prohibited_terms |   1.1         |   ops/s |
|                                                 Max Throughput |         large_prohibited_terms |   1.1         |   ops/s |
|                                        50th percentile latency |         large_prohibited_terms | 657.491       |      ms |
|                                        90th percentile latency |         large_prohibited_terms | 702.746       |      ms |
|                                        99th percentile latency |         large_prohibited_terms | 745.949       |      ms |
|                                       100th percentile latency |         large_prohibited_terms | 858.917       |      ms |
|                                   50th percentile service time |         large_prohibited_terms | 645.836       |      ms |
|                                   90th percentile service time |         large_prohibited_terms | 691.528       |      ms |
|                                   99th percentile service time |         large_prohibited_terms | 734.494       |      ms |
|                                  100th percentile service time |         large_prohibited_terms | 848.315       |      ms |
|                                                     error rate |         large_prohibited_terms |   0           |       % |
|                                                 Min Throughput |           desc_sort_population |   1.5         |   ops/s |
|                                                Mean Throughput |           desc_sort_population |   1.5         |   ops/s |
|                                              Median Throughput |           desc_sort_population |   1.5         |   ops/s |
|                                                 Max Throughput |           desc_sort_population |   1.51        |   ops/s |
|                                        50th percentile latency |           desc_sort_population |  57.7745      |      ms |
|                                        90th percentile latency |           desc_sort_population |  72.2799      |      ms |
|                                        99th percentile latency |           desc_sort_population |  83.0798      |      ms |
|                                       100th percentile latency |           desc_sort_population |  86.0525      |      ms |
|                                   50th percentile service time |           desc_sort_population |  56.2369      |      ms |
|                                   90th percentile service time |           desc_sort_population |  70.6961      |      ms |
|                                   99th percentile service time |           desc_sort_population |  81.2384      |      ms |
|                                  100th percentile service time |           desc_sort_population |  84.488       |      ms |
|                                                     error rate |           desc_sort_population |   0           |       % |
|                                                 Min Throughput |            asc_sort_population |   1.5         |   ops/s |
|                                                Mean Throughput |            asc_sort_population |   1.51        |   ops/s |
|                                              Median Throughput |            asc_sort_population |   1.51        |   ops/s |
|                                                 Max Throughput |            asc_sort_population |   1.51        |   ops/s |
|                                        50th percentile latency |            asc_sort_population |  58.6668      |      ms |
|                                        90th percentile latency |            asc_sort_population |  74.7198      |      ms |
|                                        99th percentile latency |            asc_sort_population |  87.9539      |      ms |
|                                       100th percentile latency |            asc_sort_population |  89.0465      |      ms |
|                                   50th percentile service time |            asc_sort_population |  57.1822      |      ms |
|                                   90th percentile service time |            asc_sort_population |  73.4906      |      ms |
|                                   99th percentile service time |            asc_sort_population |  85.9536      |      ms |
|                                  100th percentile service time |            asc_sort_population |  87.6887      |      ms |
|                                                     error rate |            asc_sort_population |   0           |       % |
|                                                 Min Throughput | asc_sort_with_after_population |   1.5         |   ops/s |
|                                                Mean Throughput | asc_sort_with_after_population |   1.5         |   ops/s |
|                                              Median Throughput | asc_sort_with_after_population |   1.5         |   ops/s |
|                                                 Max Throughput | asc_sort_with_after_population |   1.5         |   ops/s |
|                                        50th percentile latency | asc_sort_with_after_population |  88.4283      |      ms |
|                                        90th percentile latency | asc_sort_with_after_population | 110.419       |      ms |
|                                        99th percentile latency | asc_sort_with_after_population | 122.599       |      ms |
|                                       100th percentile latency | asc_sort_with_after_population | 125.299       |      ms |
|                                   50th percentile service time | asc_sort_with_after_population |  86.8563      |      ms |
|                                   90th percentile service time | asc_sort_with_after_population | 108.018       |      ms |
|                                   99th percentile service time | asc_sort_with_after_population | 121.315       |      ms |
|                                  100th percentile service time | asc_sort_with_after_population | 123.895       |      ms |
|                                                     error rate | asc_sort_with_after_population |   0           |       % |
|                                                 Min Throughput |            desc_sort_geonameid |   6.01        |   ops/s |
|                                                Mean Throughput |            desc_sort_geonameid |   6.01        |   ops/s |
|                                              Median Throughput |            desc_sort_geonameid |   6.01        |   ops/s |
|                                                 Max Throughput |            desc_sort_geonameid |   6.02        |   ops/s |
|                                        50th percentile latency |            desc_sort_geonameid |  10.4751      |      ms |
|                                        90th percentile latency |            desc_sort_geonameid |  12.0424      |      ms |
|                                        99th percentile latency |            desc_sort_geonameid |  12.8579      |      ms |
|                                       100th percentile latency |            desc_sort_geonameid |  14.8728      |      ms |
|                                   50th percentile service time |            desc_sort_geonameid |   9.15531     |      ms |
|                                   90th percentile service time |            desc_sort_geonameid |  10.6415      |      ms |
|                                   99th percentile service time |            desc_sort_geonameid |  11.7559      |      ms |
|                                  100th percentile service time |            desc_sort_geonameid |  13.4734      |      ms |
|                                                     error rate |            desc_sort_geonameid |   0           |       % |
|                                                 Min Throughput | desc_sort_with_after_geonameid |   6.01        |   ops/s |
|                                                Mean Throughput | desc_sort_with_after_geonameid |   6.01        |   ops/s |
|                                              Median Throughput | desc_sort_with_after_geonameid |   6.01        |   ops/s |
|                                                 Max Throughput | desc_sort_with_after_geonameid |   6.01        |   ops/s |
|                                        50th percentile latency | desc_sort_with_after_geonameid |  68.5912      |      ms |
|                                        90th percentile latency | desc_sort_with_after_geonameid |  82.8155      |      ms |
|                                        99th percentile latency | desc_sort_with_after_geonameid |  99.0937      |      ms |
|                                       100th percentile latency | desc_sort_with_after_geonameid | 100.967       |      ms |
|                                   50th percentile service time | desc_sort_with_after_geonameid |  67.2157      |      ms |
|                                   90th percentile service time | desc_sort_with_after_geonameid |  81.9984      |      ms |
|                                   99th percentile service time | desc_sort_with_after_geonameid |  97.977       |      ms |
|                                  100th percentile service time | desc_sort_with_after_geonameid | 100.074       |      ms |
|                                                     error rate | desc_sort_with_after_geonameid |   0           |       % |
|                                                 Min Throughput |             asc_sort_geonameid |   6.02        |   ops/s |
|                                                Mean Throughput |             asc_sort_geonameid |   6.02        |   ops/s |
|                                              Median Throughput |             asc_sort_geonameid |   6.02        |   ops/s |
|                                                 Max Throughput |             asc_sort_geonameid |   6.03        |   ops/s |
|                                        50th percentile latency |             asc_sort_geonameid |   7.06298     |      ms |
|                                        90th percentile latency |             asc_sort_geonameid |   8.00551     |      ms |
|                                        99th percentile latency |             asc_sort_geonameid |   8.90596     |      ms |
|                                       100th percentile latency |             asc_sort_geonameid |   8.94182     |      ms |
|                                   50th percentile service time |             asc_sort_geonameid |   5.86657     |      ms |
|                                   90th percentile service time |             asc_sort_geonameid |   7.08163     |      ms |
|                                   99th percentile service time |             asc_sort_geonameid |   7.51658     |      ms |
|                                  100th percentile service time |             asc_sort_geonameid |   8.12434     |      ms |
|                                                     error rate |             asc_sort_geonameid |   0           |       % |
|                                                 Min Throughput |  asc_sort_with_after_geonameid |   6.01        |   ops/s |
|                                                Mean Throughput |  asc_sort_with_after_geonameid |   6.02        |   ops/s |
|                                              Median Throughput |  asc_sort_with_after_geonameid |   6.02        |   ops/s |
|                                                 Max Throughput |  asc_sort_with_after_geonameid |   6.02        |   ops/s |
|                                        50th percentile latency |  asc_sort_with_after_geonameid |  66.3795      |      ms |
|                                        90th percentile latency |  asc_sort_with_after_geonameid |  75.5664      |      ms |
|                                        99th percentile latency |  asc_sort_with_after_geonameid |  97.9256      |      ms |
|                                       100th percentile latency |  asc_sort_with_after_geonameid |  98.1006      |      ms |
|                                   50th percentile service time |  asc_sort_with_after_geonameid |  64.9473      |      ms |
|                                   90th percentile service time |  asc_sort_with_after_geonameid |  74.564       |      ms |
|                                   99th percentile service time |  asc_sort_with_after_geonameid |  96.3877      |      ms |
|                                  100th percentile service time |  asc_sort_with_after_geonameid |  96.6081      |      ms |
|                                                     error rate |  asc_sort_with_after_geonameid |   0           |       % |
