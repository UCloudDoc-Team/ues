# Kibana

Kibana是一个开源的分析与可视化平台，设计出来用于和Elasticsearch一起使用的。你可以用kibana搜索、查看、交互存放在Elasticsearch索引里的数据，使用各种不同的图表、表格、地图等kibana能够很轻易地展示高级数据分析与可视化。

Kibana让我们理解大量数据变得很容易。

产品默认打开kibana服务，控制台产品页上可以点击 \`打开Kibana\`
打开kibana服务窗口。集群的kibana地址已经做了域名代理访问，服务地址不唯一，需要使用时通过控制台来打开。另外我们对kibana做了权限验证，所以不必担心开放的kibana地址会被恶意登陆操作，也请妥善保存您的账号，可以不定期修改密码确保安全。

![image](/images/operate/detail_kibana_1.jpg)

kibana打开了，你可以使用它了。

![image](/images/operate/detail_kibana_2.jpg)

[官网Kibana](https://www.elastic.co/guide/en/kibana/5.5/index.html)

## Kibana代理访问

可以通过在控制台开启或关闭Kibana代理访问，设置Kibana在公网访问是否能访问。(默认为开启状态)

**开启Kibana代理访问**

![image](/images/operate/detail_kibana_3.jpg)


**关闭Kibana代理访问**

![image](/images/operate/detail_kibana_4.jpg)

## Kibana公网访问

**应用型负载均衡ALB**

通过ALB设置防火墙白名单策略，保证公网访问Kibana安全性。

**新建集群**

2024年8月1日之后，创建UES集群时，Kibana节点独立部署，不再提供公网Kibana代理访问功能。

若客户有公网访问Kibana需求，则可以通过创建应用型负载均衡ALB, 绑定Kibana节点的IP地址、端口为5601，实现Kibana公网访问。

**原有集群**

原有集群Kibana服务部署在数据节点上，由于公网访问安全的问题，建议客户关闭Kibana代理访问，通过创建应用型负载均衡ALB, 绑定第一个数据节点的IP地址、端口为5601，实现Kibana公网访问。

