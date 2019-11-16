# rstp概述

## 传统STP的问题

我觉得Jan Ho的说明例子很明确：高延迟的网络用来提供证券交易服务 (所谓秒秒钟几十万上落)，或者用作紧急电话通讯，会容易导致因时效性过低，投入资金过多，集体资金流失的不满动荡。


## 加速的改进

* 在RSTP中只有三种端口状态，Discarding、Learning和Forwarding。
* 集成uplinkfast、backbonefast，但也只保留了配置port fast（edge port）命令功能
* 根端口和指定端口在原有基础上不变，闭塞端口非为两种:Alternate、Backup

在RSTP中将Disabled、Blocking、Listening结合为Discarding状态，不学习MAC地址，不转发数据包，接收但不发送BPDU。


### PortFast

这个是用于接入口的，一个正常的交换接口从down到up要经过:Down、listening、learning、fowarding几个状态，一共耗时为30秒，从而决定此端口是blocking还是fowarding的，也是交换机的防止环路的机制。但是对于直接接入PC这样的终端设备的接口就没有必要经过这几步了，也就是从down直接进入fowarding的状态。



### UplinkFast

当某台交换机Root Port与Bridge Priority连接中断造成Directly Fail，此时该交换机的Alternate Port由blocking转到forwarding也需要30秒时间，这个对于一个网络来说可谓完全不能接受，启动了UplinkFast的交换机会选1个 Blocking Port设为Standby，当Root Port死掉时，就绪的端口立刻转成Forwarding，省掉30秒等候时间。

### BackboneFast

Root Switch与两台互联的交换机相连，而两台互联的交换机其中一台因故障与Root Switch连接中断，虽然这一台交换机会识别出，但与它互联的另一台交换机是无法察觉的，所以依旧会等待已与Root Switch中断的交换机传来得的BPDU，Max Age过去了都收不到的话才会把BPDU弃掉，然后选择把Altn Port转成Root Port再进行连接。不过等待BPDU用了20秒，加之端口由Blocking至Forwarding又用了30秒，为此的解决方案就是BackboneFast。

原理简说就是：当开启BackboneFast的交换机发现收不到BPDU，它就会在Root Port发送用于Root Switch确认的请求包，如果Root Switch没收到，该交换机会马上进行STP运算寻找新的Root Port去往Root Switch。

### alternate port

如果一个端口收到**另外一个网桥**的更好的BPDU，但不是最好的，那么这个端口成为替换端口，如图所示。对于SW-2来说，端口P3收到的BPDU比自己优先，自己为次优先，P3为替换端口。

* 替代端口，是根端口的备份，当根端口故障后，马上成为根端口并进入转发状态

![](https://i.postimg.cc/h4y9RxQ6/460d.jpg)

### backup port

如果一个端口收到**同一个网桥**的更好 BPDU，那么这个端口成为备份端。当两个端口被一个点到点链路的一个环路连在一起时，或者当一个交换机有两个或多个到共享局域网段的连接时，一个备份端口才能存在。如图所示，SW-1的P1和P2口同时**接入到以太网的同一网段**，P1为指定端口，P2 优先级低，则P2端口为备份端口。

* 备份端口，即指定端口的备份，当指定端口故障后，会等待一段时间后成为指定端口

![](https://i.postimg.cc/5Np81RKx/a15.jpg)

> 摘抄自-[百度百科RSTP](https://baike.baidu.com/item/RSTP)


### 端口状态与过程的改良



![](https://i.postimg.cc/65THcH5q/43569.jpg)

![](https://i.postimg.cc/kGhGhvZN/57-35.png)





RSTP根据端口在活动拓扑中的作用，定义了3种端口角色（STP有5种角色）：禁用端口（Disabled Port）、根端口（Root Port）、指定端口（Designated Port）、**为支持RSTP的快速特性规定的替代端口（Alternate Port）和备份端口（Backup Port）**。




STP定义了5种不同的端口状态，关闭(disable)，监听（Listening），学习（Learning），阻断（Blocking）和转发（Forwarding）;在RSTP中只有三种端口状态，Discarding、Learning和Forwarding。802.1D中的禁止端口，监听端口，阻塞端口在802.1W中统一合并为禁止端口。

RSTP的主要功能可以归纳如下：

1. 发现并生成局域网的一个最佳树型拓扑结构
2. 发现拓扑故障并随之进行恢复，自动更新网络拓扑结构，启用备份链路，同时保持最佳树型结构

### 

https://baike.baidu.com/item/RSTP/2235256?fr=aladdin

### 从监听到学习的时间 

STP预设Listening和Learning为15秒 (Forward Delay)，端口由Down至Up，一共等待30秒。而RSTP则使用同步的概念，交换机并非被动地等待bridge priority传来的BPDU讯息再判断拓扑，取而代之的是主动去跟相邻的交换机沟通，尽快把网络拓扑传开去。这就能省却那30秒的被动学习时间，稍後详述。



写作思路

端口状态的时间转变过程

再述uplinkfast与backbonefast
