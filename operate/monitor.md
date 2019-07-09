{{indexmenu_n>5}}

## 监控告警

### 集群、集群节点监控

UES开通对集群和节点相关性能的监控，在控制台集群详情中视图展示

![image](/images/operate/detail_monitor_1.jpg)

### 告警配置

控制台上打开 **监控 UMon** 产品面板页（可用区需选择和UES一样）。**告警模板** 选择 **创建模板**
（因为UES没有默认告警模板，需要创建自定义模板）

![image](/images/operate/ues_umon_1.jpg)

如上图，创建两个自定义告警模板，集群监控项告警（ES集群）和节点监控告警项（ES集群节点），也可根据需求只创建需要的模板。

创建成功后，可在列表中看到自定义的UES告警模板

![image](/images/operate/ues_umon_2.jpg)

以节点为例，编辑模版告警规则

![image](/images/operate/ues_umon_3.jpg)

其中，可选添加规则和监控指标项相同。添加规则后，可以设置每一个告警项的告警阀值、触发周期、收敛策略、通知对象等。

模版设置完成后，可以在 **资源监控** 里，将集群节点资源配置上告警模板。

![image](/images/operate/ues_umon_4.jpg)

![image](/images/operate/ues_umon_5.jpg)

![image](/images/operate/ues_umon_6.jpg)

确定，资源添加告警成功！
