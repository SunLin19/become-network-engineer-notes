# OSPF

在了解ospf之前，先了解下内部网关协议(Interior Gateway Protocol，简称IGP)。既然是内部网关协议，路由间信息传递也是作用在大环境下的局域网范围内。IGP设计重点是针对路由的学习与交换，发现路由等，它对于路由条目的存储量是有限的（ospf最多容纳1w条路由），仅在AS内部实现路由信息的交换。

IGP又被分为两类：距离矢量路由协议和链路状态路由协议。

* 距离[矢量](https://zhidao.baidu.com/question/71933083.html)路由协议：这类协议采用**距离向量算法来决定**报文交换的路径。
* [链路](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF/7181323)状态路由协议：收集网络或该限定区域内的所有路由器信息，并且可算出每台路由器各自到达目的地的最优路径。

> 参考自 [BGP与IGP之间的区别](http://www.voidcn.com/article/p-ojocvzvo-tx.html)、[百度百科-距离矢量路由协议](https://baike.baidu.com/item/%E9%93%BE%E8%B7%AF%E7%8A%B6%E6%80%81%E8%B7%AF%E7%94%B1%E5%8D%8F%E8%AE%AE)

OSPF(Open Shortest Path First开放式最短路径优先），用于在单一自治系统（autonomous system,AS）内决策路由。是对链路状态路由协议的一种实现，隶属内部网关协议（IGP）。



写作思路

* 先用《趣学ccna》的方式作为导读
* 说明IGP与BGP的区别
* 百度百科
* 配置
