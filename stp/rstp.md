# rstp

## 传统STP的问题

我觉得Jan Ho的说明例子很明确：高延迟的网络用来提供证券交易服务 (所谓秒秒钟几十万上落)，或者用作紧急电话通讯，会容易导致因时效性过低，投入资金过多，集体资金流失的不满动荡。由STP改用RSTP所需的工夫不多，就一句`spanning-tree mode rapid-pvst`即可。

使用STP时，会见到Protocol一行显示为ieee，代表正在使用传统STP；转成RSTP后，则会显示rstp。


## 加速的改进

* 在RSTP中只有三种端口状态，Discarding、Learning和Forwarding
  * Discarding状态，不学习MAC地址，不转发数据包，接收但不发送BPDU
* 集成uplinkfast、backbonefast，但也只保留了配置port fast（edge port）命令功能
* 根端口和指定端口在原有基础上不变，闭塞端口非为两种:Alternate、Backup


在传统STP中，BPDU只会Root Switch每2秒(Hello Time) 发放一次，然后用一个传一个的方式，散播到网络上所有所有的交换机；而在RSTP中每台交换机都会发放BPDU，同样每2秒发放一次，RSTP中的BPDU有Keepalive的作用，如果Root Port过了6秒钟都收不到对放的BPDU (3 个Hello Time)，则会作出以下应变：

* 如有 Alternate Port，立刻把 Alternate Port 升级成为 Root Port
* 如没有 Alternate Port，则把自己成为 Root Switch 启动 Synchronization Process

## Port State

![](https://i.postimg.cc/85dbWyM2/81223.png)

### alternate port

如果一个端口收到**另外一个网桥**的更好的BPDU，但不是最好的，那么这个端口成为替换端口，如图所示。对于SW-2来说，端口P3收到的BPDU比自己优先，自己为次优先，P3为替换端口。

* 替代端口，是根端口的备份，当根端口故障后，马上成为根端口并进入转发状态

![](https://i.postimg.cc/h4y9RxQ6/460d.jpg)

### backup port

如果一个端口收到**同一个网桥**的更好 BPDU，那么这个端口成为备份端。当两个端口被一个点到点链路的一个环路连在一起时，或者当一个交换机有两个或多个到共享局域网段的连接时，一个备份端口才能存在。如图所示，SW-1的P1和P2口同时**接入到以太网的同一网段**，P1为指定端口，P2 优先级低，则P2端口为备份端口。

* 备份端口，即指定端口的备份，当指定端口故障后，会等待一段时间后成为指定端口

![](https://i.postimg.cc/5Np81RKx/a15.jpg)

> 摘抄自-[百度百科RSTP](https://baike.baidu.com/item/RSTP)

## link type

写作思路全双工、半双工

Link Type是RSTP新增的概念，RSTP把Link分为Edge Port、Point to Point Non-edge Port、Shared Non-edge Port。

### edge port

即是接驳主机设备的端口，沿袭了在STP中的Portfast，Edge Port设定容许该端口由Discarding直接进入Forwarding，并假设不会引致Loop而不产生TCN (Topology Change Notification)，这就避免了交换机清空mac地址表。

* Edge Port需要用`spanning-tree portfast`指令去设定。

### Point to Point Non-edge Port

RSTP会把Full Duplex Port定性为Point to Point Non-edge Port，并会用RSTP与对方进行沟通 (Synchronization Process)。

* 全双工（Full Duplex）是通讯传输的一个术语。通信允许数据在两个方向上同时传输

### Shared Non-edge Port

而Half Duplex Port则会成为Shared Non-edge Port，只能用传统STP方法沟通。不过Half Duplex Port在现今网络世界已经比较难找到了，除非网络中仍有使用旧式的HUB。

半双工（Half Duplex），所谓半双工就是指一个时间段内只有一个动作发生，举个简单例子，一条窄窄的马路，同时只能有一辆车通过，当目前有两辆车对开，这种情况下就只能一辆先过，等到头儿后另一辆再开。

## 总结

RSTP的主要功能可以归纳如下：

1. 发现并生成局域网的一个最佳树型拓扑结构
2. 发现拓扑故障并随之进行恢复，自动更新网络拓扑结构，启用备份链路，同时保持最佳树型结构

参考资料：

* [cnblogs-STP理论基础](https://www.cnblogs.com/diyudewudao/p/11773535.html)
* [百度百科-RSTP](https://baike.baidu.com/item/RSTP)
* [Jan Ho-RSTP快速生成树协定](https://www.jannet.hk/zh-Hans/post/rapid-spanning-tree-protocol-rstp)
* [51cto-Portfast /Uplinkfast/BackboneFast与RSTP的关系](https://blog.51cto.com/7658423/1663403)


