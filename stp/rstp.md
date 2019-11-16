# rstp

## 传统STP的问题

我觉得Jan Ho的说明例子很明确：高延迟的网络用来提供证券交易服务 (所谓秒秒钟几十万上落)，或者用作紧急电话通讯，会容易导致因时效性过低，投入资金过多，集体资金流失的不满动荡。


## 加速的改进

* 在RSTP中只有三种端口状态，Discarding、Learning和Forwarding。
* 集成uplinkfast、backbonefast，但也只保留了配置port fast（edge port）命令功能
* 根端口和指定端口在原有基础上不变，闭塞端口非为两种:Alternate、Backup

在RSTP中将Disabled、Blocking、Listening结合为Discarding状态，不学习MAC地址，不转发数据包，接收但不发送BPDU。


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

即是接驳主机设备的端口，沿袭了在STP中的Portfast，Edge Port设定容许该端口由Discarding直接进入Forwarding，并假设不会引致Loop而不产生TCN (Topology Change Notification)，这就避免了交换机清空mac地址表。Edge Port需要用`spanning-tree portfast`指令去设定。

### Point to Point Non-edge Port

RSTP 会把 Full Duplex Port 定性为 Point to Point Non-edge Port，并会用 RSTP 与对方进行沟通 (Synchronization Process)。

#### Shared Non-edge Port

而 Half Duplex Port 则会成为 Shared Non-edge Port，只能用传统 STP 方法沟通。不过 Half Duplex Port 在现今网络世界已经比较难找到了，除非网络中仍有使用旧式的 HUB。

### 总结

RSTP的主要功能可以归纳如下：

1. 发现并生成局域网的一个最佳树型拓扑结构
2. 发现拓扑故障并随之进行恢复，自动更新网络拓扑结构，启用备份链路，同时保持最佳树型结构

### 

https://baike.baidu.com/item/RSTP/2235256?fr=aladdin
