# OSPF

在了解ospf之前，先了解下内部网关协议(Interior Gateway Protocol，简称IGP)。既然是内部网关协议，路由间信息传递也是作用在大环境下的局域网范围内。IGP设计重点是针对路由的学习与交换，发现路由等，它对于路由条目的存储量是有限的（ospf最多容纳1w条路由），仅在AS内部实现路由信息的交换。

IGP又被分为两类：距离矢量路由协议和链路状态路由协议。

* 距离[矢量](https://zhidao.baidu.com/question/71933083.html)路由协议：这类协议采用**距离向量算法来决定**报文交换的路径。
* [链路](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF/7181323)状态路由协议：收集网络或该限定区域内的所有路由器信息，并且可算出每台路由器各自到达目的地的最优路径。

> 参考自 [BGP与IGP之间的区别](http://www.voidcn.com/article/p-ojocvzvo-tx.html)、[百度百科-距离矢量路由协议](https://baike.baidu.com/item/%E9%93%BE%E8%B7%AF%E7%8A%B6%E6%80%81%E8%B7%AF%E7%94%B1%E5%8D%8F%E8%AE%AE)

OSPF(Open Shortest Path First开放式最短路径优先），用于在单一自治系统（autonomous system,AS）内决策路由。是对链路状态路由协议的一种实现，隶属内部网关协议（IGP）。整个作业流程使用了五种报文、三个阶段、四张表。

## ospf作业流程

五种报文
* Hello报文：建立并维护邻居关系。
* DBD报文：发送链路状态头部信息。
* LSR报文：把从DBD中找出需要的链路状态头部信息传给邻居，请求完整信息。
* LSU报文：将LSR请求的头部信息对应的完整信息发给邻居。
* LSACK：收到LSU报文后确认该报文。

三个阶段
1. 邻居发现：通过发送Hello报文形成邻居关系。 
1. 路由通告：邻居间发送链路状态信息形成邻接关系。 
1. 路由计算：根据最短路径算法（这个算法理解难度很高）算出路由表。 

四张表

|表|作用
|:-:|:-:
|邻居表|主要记录形成邻居关系路由器
|链路状态数据库|记录链路状态信息 
|OSPF路由表|通过链路状态数据库得出 
|全局路由表|OSPF路由与其他比较得出

ospf了解自身链路，寻找邻居并与其他路由器建立邻居关系后，路由器就可以创建链路状态数据包将描述链路状态的LSA泛洪到邻居，最终形成包含网络完整链路状态信息的链路状态数据库，路由区域内的每台路由器使用SPF算法来独立计算各个路由的最优路径并传输数据信息。

> 摘抄自[百度百科-组播扩展](https://baike.baidu.com/item/组播扩展OSPF)并参考 [晴刃-OSPF协议详解](https://www.qingsword.com/qing/596.html)

## ospf选举

在了解ospf选举之前，需要了解以下名词：

* DR和BDR
* nbma网络

### DR和BDR

运行OSPF的路由器通过与邻居路由器建立邻接关系，互相传递链路状态信息。如果每两个路由器之间都要建立邻接关系，那么就会构成n(n-1)/2个邻接关系，这时就有些混乱了，而且浪费了很多不必要的网络资源。

![](https://i.postimg.cc/sg4BRQ2B/Snipaste-2019-10-26-10-56-48.png)

为了可以避免这些问题的发生，可以在该网段上选举一个指定路由器（Designated Router）。由DR和网络中的其他路由器建立邻接关系，并负责将网段上的变化告知它们。

为了实现冗余，当DR失效时，需要有一个新的DR来接替DR的工作，这便是BDR（Backup Designated Router）。网络上所有的路由器将和DR和BDR同时形成邻接关系。DR和BDR之间也将形成邻接关系。

如图所示，用红蓝线代表建立的邻接关系。可以看到采用DR/BDR机制，5台路由器之间只需建立7个邻接关系就可以了。

![](https://i.postimg.cc/9f43bcd0/Snipaste-2019-10-26-07-14-16.png)

> 参考 [51cto-路由器OSPF的基本概念与工作过程](https://blog.51cto.com/14154700/2374461)

### NBMA

非广播-多路访问网络（Non-Broadcast Multiple Access）是OSPF通信协议中定义的四种网络类型的其中一种。它用来描述不具有支持广播和多播能力的多路访问网络。其他OSPF定义的网络类型有：广播、点对点、点对多点网络。

![](https://i.postimg.cc/Hnx97j06/Snipaste-2019-10-25-22-43-06.png)

在配置NBMA的情况下，OSPF在同一时刻向每一个路由器只会周期性发送一个Hello包，用于建立和确定路由间的邻居关系而不是进行多播。需要注意的是只有在广播或NBMA类型接口才会选举,在点到点或点到多点类型的接口上不需要选举。

>参考 [广播类型的ospf网络简介-dr和bdr](https://hexnet.jimdo.com/2012/11/14/%E5%B9%BF%E6%92%AD%E7%B1%BB%E5%9E%8B%E7%9A%84ospf%E7%BD%91%E7%BB%9C%E7%AE%80%E4%BB%8B-dr%E5%92%8Cbdr/)、[百度百科-NBMA](https://baike.baidu.com/item/NBMA)

### DR和BDR选举过程

OSPF使用优先级值选择DR和BDR。优先级最高的OSPF路由器将成为DR。具有第二高优先级的路由器成为BDR。如果有平局，将选择具有最高RID的路由器。

优先级值为8位。默认优先级值为1。我们可以设置0到255之间的任何值。可以使用ip ospf priority命令从接口子配置模式更改它。

我们可以通过更改其优先级值来强制使任何路由器成为DR（最高）或BDR（第二高）。如果将优先级值设置为0，它将永远不会成为DR或BDR。

#### 条件1：- 使用最高优先级值

例如，下图说明了一个简单的OPSF网络。在这个网络中，我们有五个路由器。我们不希望R3成为DR或BDR。因此，我们将其默认优先级值更改为0。现在让我们看看这些路由器如何选择DR和BDR。

![](https://i.postimg.cc/prL8p6fz/ospf-dr-bdr.png)

#### 条件2：- 如果有平局，使用最高的RID

我们的路由器将使用RID来选择DR和BDR。从高到低排列路由器将为我们提供DR和BDR。众所周知，网络有两种：不需要DR和BDR进行交换过程的网络，以及需要DR和BDR进行交换过程的网络。

* 在第一种类型中，所有路由器将彼此交换路由信息
* 在第二种类型中，DR OTHERs将与DR和BDR交换路由信息

将交换路由信息的路由器称为相邻路由器，两个相邻的关系称为邻接关系。该术语与接口相关联，具有两个接口的路由器可以在一个接口中相邻，而在另一个接口中DR OTHER。

例如，下图说明了运行NBMA网络的OSPF。在这个网络中：

* R3将与R1建立邻接关系，因此在这种关系中，它们将被视为**Adjacent**
* R3不会与R4建立邻接关系，因此在这种关系下，它们将仅被视为**DR OTHER**
* 在不需要DR和BDR的网络中，所有路由器都将被视为**Adjacent**，它们之间的关系将被视为**Adjacency**
* 只有相邻的路由器才会进入下一状态以建立邻接关系

![](https://i.postimg.cc/fLrWfq4x/ospf-adjacent-adjacency.png)


> 摘抄自 [OSPF Neighbor States Explained with Example](https://www.computernetworkingnotes.com/ccna-study-guide/ospf-neighbor-states-explained-with-example.html)

## OSPF协议优缺点

优点

* 避免路由环路
* 收敛速度快
* 组播触发式更新
* 将协议自身的开销控制到最小

缺点：配置相对复杂、负载均衡较弱

OSPF非常适合企业级网络，而EIGRP思科自有属性太重，最多支持也只有256跳路由。
