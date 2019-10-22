# OSPF

OSPF(Open Shortest Path First开放式最短路径优先）是一个内部网关协议(Interior Gateway Protocol，简称IGP），用于在单一自治系统（autonomous system,AS）内决策路由。是对链路状态路由协议的一种实现，隶属内部网关协议（IGP）。

> 摘自 [百度百科-OSPF](https://baike.baidu.com/item/%E7%BB%84%E6%92%AD%E6%89%A9%E5%B1%95OSPF)

IGP（内部网关协议）对于路由条目的存储量是有限的（ospf最多容纳1w条路由），仅在AS内部实现路由信息的交换；IGP设计重点是针对路由的学习与交换，发现路由等；IGP又被分为两类：距离矢量路由协议和链路状态路由协议。

> 参考自 [BGP与IGP之间的区别](http://www.voidcn.com/article/p-ojocvzvo-tx.html)


距离矢量路由协议：这类协议采用**距离向量算法来决定报文交换的路径**。



链路状态路由协议：

写作思路

* 先用《趣学ccna》的方式作为导读
* 说明IGP与BGP的区别
* 百度百科
* 配置
