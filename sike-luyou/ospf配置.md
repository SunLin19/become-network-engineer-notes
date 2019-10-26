写作思路



## ospf选举

在了解ospf选举之前，需要了解以下名词：

* DR和BDR
* nbma网络

### DR和BDR

![](https://i.postimg.cc/sg4BRQ2B/Snipaste-2019-10-26-10-56-48.png)

运行OSPF的路由器通过与邻居路由器建立邻接关系，互相传递链路状态信息。如果每两个路由器之间都要建立邻接关系，那么就会构成n(n-1)/2个邻接关系，这时就有些混乱了，而且浪费了很多不必要的网络资源。

为了可以避免这些问题的发生，可以在该网段上选举一个指定路由器（Designated Router）。由DR和网络中的其他路由器建立邻接关系，并负责将网段上的变化告知它们。

为了实现冗余，当DR失效时，需要有一个新的DR来接替DR的工作，这便是BDR（Backup Designated Router）。网络上所有的路由器将和DR和BDR同时形成邻接关系。DR和BDR之间也将形成邻接关系。

如图所示，用红蓝线代表建立的邻接关系。可以看到采用DR/BDR机制，5台路由器之间只需建立7个邻接关系就可以了。

![](https://i.postimg.cc/9f43bcd0/Snipaste-2019-10-26-07-14-16.png)

> 参考 [51cto-路由器OSPF的基本概念与工作过程](https://blog.51cto.com/14154700/2374461)

### nbma

![](https://i.postimg.cc/Hnx97j06/Snipaste-2019-10-25-22-43-06.png)

非广播-多路访问网络（Non-Broadcast Multiple Access）是OSPF通信协议中定义的四种网络类型的其中一种。它用来描述不具有支持广播和多播能力的多路访问网络。其他OSPF定义的网络类型有：广播、点对点、点对多点网络。

在配置NBMA的情况下，OSPF在同一时刻向每一个路由器只会周期性发送一个Hello包，用于建立和确定路由间的邻居关系而不是进行多播。需要注意的是只有在广播或NBMA类型接口才会选举,在点到点或点到多点类型的接口上不需要选举。

>参考 [广播类型的ospf网络简介-dr和bdr](https://hexnet.jimdo.com/2012/11/14/%E5%B9%BF%E6%92%AD%E7%B1%BB%E5%9E%8B%E7%9A%84ospf%E7%BD%91%E7%BB%9C%E7%AE%80%E4%BB%8B-dr%E5%92%8Cbdr/)、[百度百科-NBMA](https://baike.baidu.com/item/NBMA)



https://blog.51cto.com/14154700/2374461



## 实验拓扑

![](https://i.postimg.cc/50Vmgf6x/Snipaste-2019-10-24-22-34-22.png)

> 拓扑参考自 [基于点到点链路的 OSPF 的初始配置](https://www.cisco.com/c/zh_cn/support/docs/ip/open-shortest-path-first-ospf/13687-15.html?dtid=osscdc000357)

Router0

```ios
Router(config)#interface loopback 0
Router(config-if)#ip add 3.3.3.3 255.255.255.255
Router(config-if)#interface s0/1/0
Router(config-if)#ip add 1.1.1.2 255.255.255.0
Router(config-if)#no shutdown
Router(config)#router ospf 1
Router(config-router)#1.1.1.0 0.0.0.255 area 0
```

Router1

```ios
Router(config)#interface loopback 0
Router(config-if)#ip add 2.2.2.2 255.255.255.255
Router(config-if)#int s0/1/1
Router(config-if)#ip add 1.1.1.1 255.255.255.0
Router(config-if)#clock rate 64000
Router(config-if)#router ospf 1
Router(config-router)#network 1.1.1.0 0.0.0.255 area 0
Router(config-if)#no shutdown
```

## clock rate 64000

要想知道配置为什么时钟频率，首先得要知道什么是DCE、DTE？

* **DCE称DataCircuit-terminatingEquipment，数字通信设备**，例如：路由器。
* **DTE英文全称DataTerminalEquipment，数字终端设备**，指一般的终端或是计算机。

DTE与DCE最为明显的区别：DCE一方提供时钟，DTE不提供时钟，但它依靠DCE提供的时钟工作。btw，路由器既可以作为DCE，也可以作为DTE。


关于`Clock rate 64000`的解释：思科建议64000这个值在它默认的信号类型的线上做设置，而更高的值需要其它类型线缆支持，速率设置超过实际物理线缆所能承受的话，可能会发生丢包等问题。

> 参考自 [cisco-clock rate Usage Guidelines](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/interface/command/ir-cr-book/ir-c2.html#wp3930272930)

改进

https://www.cisco.com/c/zh_cn/support/docs/ip/open-shortest-path-first-ospf/47862-ospfdb3.html


配置

https://jingyan.baidu.com/article/636f38bb4d8ff7d6b84610ba.html

https://www.cisco.com/c/zh_cn/support/docs/ip/open-shortest-path-first-ospf/118879-configure-ospf-00.html?dtid=osscdc000357

https://blog.51cto.com/14157628/2392133

https://blog.51cto.com/yangshufan/1961182



