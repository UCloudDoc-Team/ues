# Rally压测

我们利用 Elasticsearch 官方提供的 benchmark 压测工具
\[rally\](<https://github.com/elastic/rally>)
对华北（北京）可用区B的Elasticsearch集群（V5.5.1）进行测试，测试结果如下：

**8核16G 3个节点**

| Metric                      | Operation                  | Value     | Unit  |
| --------------------------- | -------------------------- | --------- | ----- |
| Indexing time               |                            | 31.7303   | min   |
| Merge time                  |                            | 9.5995    | min   |
| Refresh time                |                            | 1.93053   | min   |
| Flush time                  |                            | 0.0881167 | min   |
| Merge throttle time         |                            | 1.54493   | min   |
| Total Young Gen GC          |                            | 58.079    | s     |
| Total Old Gen GC            |                            | 0.196     | s     |
| Heap used for segments      |                            | 18.3168   | MB    |
| Heap used for doc values    |                            | 0.103638  | MB    |
| Heap used for terms         |                            | 17.1259   | MB    |
| Heap used for norms         |                            | 0.0717773 | MB    |
| Heap used for points        |                            | 0.217482  | MB    |
| Heap used for stored fields |                            | 0.797966  | MB    |
| Segment count               |                            | 94        |       |
| Min Throughput              | index-stats                | 100.045   | ops/s |
| Median Throughput           | index-stats                | 100.068   | ops/s |
| Max Throughput              | index-stats                | 100.125   | ops/s |
| 50th percentile latency     | index-stats                | 3.61395   | ms    |
| 90th percentile latency     | index-stats                | 4.04719   | ms    |
| 99th percentile latency     | index-stats                | 4.96298   | ms    |
| 99.9th percentile latency   | index-stats                | 16.8928   | ms    |
| 100th percentile latency    | index-stats                | 23.5338   | ms    |
| Min Throughput              | node-stats                 | 100.031   | ops/s |
| Median Throughput           | node-stats                 | 100.09    | ops/s |
| Max Throughput              | node-stats                 | 100.506   | ops/s |
| 50th percentile latency     | node-stats                 | 4.48539   | ms    |
| 90th percentile latency     | node-stats                 | 5.0753    | ms    |
| 99th percentile latency     | node-stats                 | 7.10029   | ms    |
| 99.9th percentile latency   | node-stats                 | 17.5065   | ms    |
| 100th percentile latency    | node-stats                 | 23.1605   | ms    |
| Min Throughput              | default                    | 49.9519   | ops/s |
| Median Throughput           | default                    | 50.013    | ops/s |
| Max Throughput              | default                    | 50.0216   | ops/s |
| 50th percentile latency     | default                    | 15.2663   | ms    |
| 90th percentile latency     | default                    | 16.0737   | ms    |
| 99th percentile latency     | default                    | 18.9459   | ms    |
| 99.9th percentile latency   | default                    | 40.3196   | ms    |
| 100th percentile latency    | default                    | 45.0553   | ms    |
| Min Throughput              | term                       | 200.063   | ops/s |
| Median Throughput           | term                       | 200.091   | ops/s |
| Max Throughput              | term                       | 200.174   | ops/s |
| 50th percentile latency     | term                       | 2.59966   | ms    |
| 90th percentile latency     | term                       | 3.12504   | ms    |
| 99th percentile latency     | term                       | 31.4645   | ms    |
| 99.9th percentile latency   | term                       | 43.7732   | ms    |
| 100th percentile latency    | term                       | 45.5124   | ms    |
| Min Throughput              | phrase                     | 200.03    | ops/s |
| Median Throughput           | phrase                     | 200.049   | ops/s |
| Max Throughput              | phrase                     | 200.075   | ops/s |
| 50th percentile latency     | phrase                     | 3.88241   | ms    |
| 90th percentile latency     | phrase                     | 5.3865    | ms    |
| 99th percentile latency     | phrase                     | 44.4824   | ms    |
| 99.9th percentile latency   | phrase                     | 54.733    | ms    |
| 100th percentile latency    | phrase                     | 55.2758   | ms    |
| Min Throughput              | country\\\_agg\\\_uncached | 2.75134   | ops/s |
| Median Throughput           | country\\\_agg\\\_uncached | 2.76574   | ops/s |
| Max Throughput              | country\\\_agg\\\_uncached | 2.8003    | ops/s |
| 50th percentile latency     | country\\\_agg\\\_uncached | 162393    | ms    |
| 90th percentile latency     | country\\\_agg\\\_uncached | 222884    | ms    |
| 99th percentile latency     | country\\\_agg\\\_uncached | 234288    | ms    |
| 99.9th percentile latency   | country\\\_agg\\\_uncached | 235730    | ms    |
| 100th percentile latency    | country\\\_agg\\\_uncached | 235940    | ms    |
| Min Throughput              | country\\\_agg\\\_cached   | 100.051   | ops/s |
| Median Throughput           | country\\\_agg\\\_cached   | 100.073   | ops/s |
| Max Throughput              | country\\\_agg\\\_cached   | 100.138   | ops/s |
| 50th percentile latency     | country\\\_agg\\\_cached   | 2.87947   | ms    |
| 90th percentile latency     | country\\\_agg\\\_cached   | 3.31356   | ms    |
| 99th percentile latency     | country\\\_agg\\\_cached   | 6.88291   | ms    |
| 99.9th percentile latency   | country\\\_agg\\\_cached   | 30.7471   | ms    |
| 100th percentile latency    | country\\\_agg\\\_cached   | 37.4828   | ms    |
| Min Throughput              | scroll                     | 35.6468   | ops/s |
| Median Throughput           | scroll                     | 35.81     | ops/s |
| Max Throughput              | scroll                     | 35.8374   | ops/s |
| 50th percentile latency     | scroll                     | 296588    | ms    |
| 90th percentile latency     | scroll                     | 427849    | ms    |
| 99th percentile latency     | scroll                     | 457200    | ms    |
| 100th percentile latency    | scroll                     | 460396    | ms    |
| Min Throughput              | expression                 | 1.28785   | ops/s |
| Median Throughput           | expression                 | 1.31216   | ops/s |
| Max Throughput              | expression                 | 1.31935   | ops/s |
| 50th percentile latency     | expression                 | 78301.4   | ms    |
| 90th percentile latency     | expression                 | 99426.7   | ms    |
| 99th percentile latency     | expression                 | 103425    | ms    |
| 100th percentile latency    | expression                 | 103679    | ms    |

**2核8G 3个节点**

| Metric                      | Operation                  | Value     | Unit  |
| --------------------------- | -------------------------- | --------- | ----- |
| Indexing time               |                            | 43.8383   | min   |
| Merge time                  |                            | 22.5664   | min   |
| Refresh time                |                            | 9.2856    | min   |
| Flush time                  |                            | 0.0502833 | min   |
| Merge throttle time         |                            | 2.30762   | min   |
| Total Young Gen GC          |                            | 418.967   | s     |
| Total Old Gen GC            |                            | 3.078     | s     |
| Heap used for segments      |                            | 17.7271   | MB    |
| Heap used for doc values    |                            | 0.102936  | MB    |
| Heap used for terms         |                            | 16.5306   | MB    |
| Heap used for norms         |                            | 0.071167  | MB    |
| Heap used for points        |                            | 0.209009  | MB    |
| Heap used for stored fields |                            | 0.813393  | MB    |
| Segment count               |                            | 96        |       |
| Min Throughput              | index-stats                | 100.035   | ops/s |
| Median Throughput           | index-stats                | 100.064   | ops/s |
| Max Throughput              | index-stats                | 100.099   | ops/s |
| 50th percentile latency     | index-stats                | 3.98357   | ms    |
| 90th percentile latency     | index-stats                | 5.42446   | ms    |
| 99th percentile latency     | index-stats                | 17.3357   | ms    |
| 99.9th percentile latency   | index-stats                | 38.0944   | ms    |
| 100th percentile latency    | index-stats                | 44.4198   | ms    |
| Min Throughput              | node-stats                 | 100.011   | ops/s |
| Median Throughput           | node-stats                 | 100.089   | ops/s |
| Max Throughput              | node-stats                 | 100.539   | ops/s |
| 50th percentile latency     | node-stats                 | 4.45035   | ms    |
| 90th percentile latency     | node-stats                 | 6.89122   | ms    |
| 99th percentile latency     | node-stats                 | 19.1262   | ms    |
| 99.9th percentile latency   | node-stats                 | 39.2981   | ms    |
| 100th percentile latency    | node-stats                 | 44.4663   | ms    |
| Min Throughput              | default                    | 25.5403   | ops/s |
| Median Throughput           | default                    | 26.1976   | ops/s |
| Max Throughput              | default                    | 28.0422   | ops/s |
| 50th percentile latency     | default                    | 18200.4   | ms    |
| 90th percentile latency     | default                    | 26643.4   | ms    |
| 99th percentile latency     | default                    | 28551     | ms    |
| 99.9th percentile latency   | default                    | 28734.5   | ms    |
| 100th percentile latency    | default                    | 28751.2   | ms    |
| Min Throughput              | term                       | 198.796   | ops/s |
| Median Throughput           | term                       | 200.081   | ops/s |
| Max Throughput              | term                       | 200.118   | ops/s |
| 50th percentile latency     | term                       | 2.69169   | ms    |
| 90th percentile latency     | term                       | 15.5542   | ms    |
| 99th percentile latency     | term                       | 52.1687   | ms    |
| 99.9th percentile latency   | term                       | 62.2191   | ms    |
| 100th percentile latency    | term                       | 63.3586   | ms    |
| Min Throughput              | phrase                     | 196.082   | ops/s |
| Median Throughput           | phrase                     | 200.032   | ops/s |
| Max Throughput              | phrase                     | 200.049   | ops/s |
| 50th percentile latency     | phrase                     | 4.70513   | ms    |
| 90th percentile latency     | phrase                     | 41.5631   | ms    |
| 99th percentile latency     | phrase                     | 61.2904   | ms    |
| 99.9th percentile latency   | phrase                     | 64.1431   | ms    |
| 100th percentile latency    | phrase                     | 64.8171   | ms    |
| Min Throughput              | country\\\_agg\\\_uncached | 2.53125   | ops/s |
| Median Throughput           | country\\\_agg\\\_uncached | 2.55542   | ops/s |
| Max Throughput              | country\\\_agg\\\_uncached | 2.56809   | ops/s |
| 50th percentile latency     | country\\\_agg\\\_uncached | 191703    | ms    |
| 90th percentile latency     | country\\\_agg\\\_uncached | 265937    | ms    |
| 99th percentile latency     | country\\\_agg\\\_uncached | 282614    | ms    |
| 99.9th percentile latency   | country\\\_agg\\\_uncached | 284102    | ms    |
| 100th percentile latency    | country\\\_agg\\\_uncached | 284297    | ms    |
| Min Throughput              | country\\\_agg\\\_cached   | 100.046   | ops/s |
| Median Throughput           | country\\\_agg\\\_cached   | 100.072   | ops/s |
| Max Throughput              | country\\\_agg\\\_cached   | 100.134   | ops/s |
| 50th percentile latency     | country\\\_agg\\\_cached   | 3.24472   | ms    |
| 90th percentile latency     | country\\\_agg\\\_cached   | 4.16849   | ms    |
| 99th percentile latency     | country\\\_agg\\\_cached   | 25.1214   | ms    |
| 99.9th percentile latency   | country\\\_agg\\\_cached   | 43.7322   | ms    |
| 100th percentile latency    | country\\\_agg\\\_cached   | 50.3143   | ms    |
| Min Throughput              | scroll                     | 31.3553   | ops/s |
| Median Throughput           | scroll                     | 31.6427   | ops/s |
| Max Throughput              | scroll                     | 31.7946   | ops/s |
| 50th percentile latency     | scroll                     | 337930    | ms    |
| 90th percentile latency     | scroll                     | 485487    | ms    |
| 99th percentile latency     | scroll                     | 518748    | ms    |
| 100th percentile latency    | scroll                     | 522454    | ms    |
| Min Throughput              | expression                 | 1.09803   | ops/s |
| Median Throughput           | expression                 | 1.11245   | ops/s |
| Max Throughput              | expression                 | 1.11853   | ops/s |
| 50th percentile latency     | expression                 | 120302    | ms    |
| 90th percentile latency     | expression                 | 150630    | ms    |
| 99th percentile latency     | expression                 | 157380    | ms    |
| 100th percentile latency    | expression                 | 158111    | ms    |

**4核8G 3个节点**

| Metric                      | Operation                  | Value     | Unit  |
| --------------------------- | -------------------------- | --------- | ----- |
| Indexing time               |                            | 30.2766   | min   |
| Merge time                  |                            | 11.1005   | min   |
| Refresh time                |                            | 3.1837    | min   |
| Flush time                  |                            | 0.0554    | min   |
| Merge throttle time         |                            | 1.49132   | min   |
| Total Young Gen GC          |                            | 87.16     | s     |
| Total Old Gen GC            |                            | 2.738     | s     |
| Heap used for segments      |                            | 17.9447   | MB    |
| Heap used for doc values    |                            | 0.0973434 | MB    |
| Heap used for terms         |                            | 16.7564   | MB    |
| Heap used for norms         |                            | 0.0637817 | MB    |
| Heap used for points        |                            | 0.220229  | MB    |
| Heap used for stored fields |                            | 0.806961  | MB    |
| Segment count               |                            | 84        |       |
| Min Throughput              | index-stats                | 99.9043   | ops/s |
| Median Throughput           | index-stats                | 100.055   | ops/s |
| Max Throughput              | index-stats                | 100.111   | ops/s |
| 50th percentile latency     | index-stats                | 4.31123   | ms    |
| 90th percentile latency     | index-stats                | 5.01966   | ms    |
| 99th percentile latency     | index-stats                | 6.73182   | ms    |
| 99.9th percentile latency   | index-stats                | 16.3152   | ms    |
| 100th percentile latency    | index-stats                | 16.7211   | ms    |
| Min Throughput              | node-stats                 | 99.9997   | ops/s |
| Median Throughput           | node-stats                 | 100.068   | ops/s |
| Max Throughput              | node-stats                 | 100.441   | ops/s |
| 50th percentile latency     | node-stats                 | 5.41895   | ms    |
| 90th percentile latency     | node-stats                 | 6.62956   | ms    |
| 99th percentile latency     | node-stats                 | 10.4876   | ms    |
| 99.9th percentile latency   | node-stats                 | 16.8256   | ms    |
| 100th percentile latency    | node-stats                 | 22.3502   | ms    |
| Min Throughput              | default                    | 49.9808   | ops/s |
| Median Throughput           | default                    | 50.016    | ops/s |
| Max Throughput              | default                    | 50.0348   | ops/s |
| 50th percentile latency     | default                    | 14.0526   | ms    |
| 90th percentile latency     | default                    | 16.2503   | ms    |
| 99th percentile latency     | default                    | 21.8674   | ms    |
| 99.9th percentile latency   | default                    | 29.5538   | ms    |
| 100th percentile latency    | default                    | 31.5155   | ms    |
| Min Throughput              | term                       | 199.745   | ops/s |
| Median Throughput           | term                       | 200.06    | ops/s |
| Max Throughput              | term                       | 200.087   | ops/s |
| 50th percentile latency     | term                       | 3.06953   | ms    |
| 90th percentile latency     | term                       | 4.04719   | ms    |
| 99th percentile latency     | term                       | 24.0726   | ms    |
| 99.9th percentile latency   | term                       | 37.8865   | ms    |
| 100th percentile latency    | term                       | 38.5456   | ms    |
| Min Throughput              | phrase                     | 199.35    | ops/s |
| Median Throughput           | phrase                     | 199.992   | ops/s |
| Max Throughput              | phrase                     | 200.071   | ops/s |
| 50th percentile latency     | phrase                     | 4.59871   | ms    |
| 90th percentile latency     | phrase                     | 22.3203   | ms    |
| 99th percentile latency     | phrase                     | 48.3212   | ms    |
| 99.9th percentile latency   | phrase                     | 52.8152   | ms    |
| 100th percentile latency    | phrase                     | 52.8472   | ms    |
| Min Throughput              | country\\\_agg\\\_uncached | 4.49216   | ops/s |
| Median Throughput           | country\\\_agg\\\_uncached | 4.67822   | ops/s |
| Max Throughput              | country\\\_agg\\\_uncached | 4.7315    | ops/s |
| 50th percentile latency     | country\\\_agg\\\_uncached | 14575.8   | ms    |
| 90th percentile latency     | country\\\_agg\\\_uncached | 17213.5   | ms    |
| 99th percentile latency     | country\\\_agg\\\_uncached | 18765.9   | ms    |
| 99.9th percentile latency   | country\\\_agg\\\_uncached | 18810.1   | ms    |
| 100th percentile latency    | country\\\_agg\\\_uncached | 18813.6   | ms    |
| Min Throughput              | country\\\_agg\\\_cached   | 100.046   | ops/s |
| Median Throughput           | country\\\_agg\\\_cached   | 100.06    | ops/s |
| Max Throughput              | country\\\_agg\\\_cached   | 100.129   | ops/s |
| 50th percentile latency     | country\\\_agg\\\_cached   | 3.49425   | ms    |
| 90th percentile latency     | country\\\_agg\\\_cached   | 4.68459   | ms    |
| 99th percentile latency     | country\\\_agg\\\_cached   | 6.65144   | ms    |
| 99.9th percentile latency   | country\\\_agg\\\_cached   | 17.7776   | ms    |
| 100th percentile latency    | country\\\_agg\\\_cached   | 24.6469   | ms    |
| Min Throughput              | scroll                     | 32.6045   | ops/s |
| Median Throughput           | scroll                     | 32.725    | ops/s |
| Max Throughput              | scroll                     | 32.7822   | ops/s |
| 50th percentile latency     | scroll                     | 326129    | ms    |
| 90th percentile latency     | scroll                     | 470066    | ms    |
| 99th percentile latency     | scroll                     | 502268    | ms    |
| 100th percentile latency    | scroll                     | 505901    | ms    |
| Min Throughput              | expression                 | 1.89188   | ops/s |
| Median Throughput           | expression                 | 1.90472   | ops/s |
| Max Throughput              | expression                 | 1.94701   | ops/s |
| 50th percentile latency     | expression                 | 7336.16   | ms    |
| 90th percentile latency     | expression                 | 8155.22   | ms    |
| 99th percentile latency     | expression                 | 8272.59   | ms    |
| 100th percentile latency    | expression                 | 8322.61   | ms    |