# 集群操作

集群相关操作在列表页和详情页都有入口，操作的对象为集群和集群节点。

## 集群的启、关、删

![image](/images/deal_service_1.jpg)

**以关闭服务为例说明**

![image](/images/operate/deal_service_stop_1.jpg)
![image](/images/operate/deal_service_stop_2.jpg)
![image](/images/operate/deal_service_stop_3.jpg)

启动、关闭、重启操作服务状态会动态改变，直至操作结束。在集群列表这种动态变化也会展现。

删除操作该集群将从列表中除去。

目前不支持批量操作，现有批量选择只会默认选中第一项进行操作。

**以上操作请谨慎操作，对已在生产使用的集群可能会造成重大影响。此操作对新建或无数据的集群比较友善**

## 集群扩容

![image](/images/operate/deal_service_resize_1.jpg)

其中节点配置不能进行选择，默认为初始创建集群所选节点类型。

需要注意的是目前只支持扩容，即添加集群节点，暂不支持缩减节点。

注意扩容提示项 "节点扩容到"
为扩容后集群节点个数，所以限制最小值为原集群节点个数。其中新增节点个数不能大于节点配额，我们分配的节点配额足够支撑你的集群使用，如果配额需求过大，请联系客户经理进行节点资源配额调整。

计费类型为初始创建集群所选计费类型，如有续费、变更计费方式，请在控制台用户中心进行查询修改。

## 集群配置管理

详情页集群概况包含 **参数管理** 操作

![image](/images/operate/detail_cfg_manager_1.jpg)

![image](/images/operate/detail_cfg_manager_2.jpg)

打开参数管理如上图所示。其中列出参数名称、参数值、应用范围、参数说明等项。另外提供参数名称搜索快速检索修改。

值得注意的是，其中的很多参数，我们已经根据所选集群节点配置做出了合理的初始化。除非是自己业务需求必须修改某些参数，其他情况不建议修改，另外修改后的参数值一定要符合规范，否则可能会对集群产生不友好的影响。修改参数后需重启集群服务以生效。

对于未被列出的参数修改，大多数是可以通过API动态修改的，我们也提倡尽可能使用API来动态修改配置参数。

## 集群线性重启

假设生产环境某种情况下需要重启集群，但是要求不能影响服务的正常使用。这时候集群的线性重启就显得尤为重要。

![image](/images/operate/detail_nodelist_2.jpg)

详情节点列表中提供了单节点重启的操作，如上图。进行重启单节点操作

![image](/images/operate/deal_service_noderestart_1.jpg)

![image](/images/operate/deal_service_noderestart_2.jpg)

重启成功会有上图提示。

值得注意的是，**单节点重启成功后，集群需要一定时间恢复到健康状态，之后再进行下一个节点的操作**

检测集群是否恢复健康的方法：

\* 集群概况中刷新集群信息模块，**一定要刷新来重新获取集群状态**

![image](/images/operate/deal_service_noderestart_3.jpg)

刷新一次后，如果状态变为黄色，可停止刷新，等待集群自动恢复到绿色健康状态；如果状态为绿色，说明集群已恢复健康状态；如果状态显示消失，请持续间隔刷新，直到集群状态显示。当集群为绿色状态时，可进行下一个节点操作。

\* 通过API \`/\_cluster/health\` 获取实时集群状态，直到集群状态恢复健康状态。

## 节点日志查询

详情页节点列表包含 **节点日志** 操作

![image](/images/operate/detail_logs_1.jpg)

初次打开默认为当前小时，并且提供选择时间段来查询节点的输出日志。

## kibana密码重置

![image](/images/operate/deal_service_reset_passwd_1.jpg)
![image](/images/operate/deal_service_reset_passwd_2.jpg)

忘记kibana密码可以在控制台来重置密码，kibana账号目前不支持修改。如果忘记账号，请联系客户经理找回。