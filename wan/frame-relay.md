# 帧中继

## 帧中继概述

电信运营商一般提供三种接入服务：互联网访问、二层专线访问、三层VPN访问

普通用户通常访问互联网，运营商给用户提供上网的设备，用户动态拨号上网，这个没太多的说法。

企业网络（专线）与家庭宽带体验差别：专线顾名思义，可以理解为一条企业专属线路，而家庭宽带则是公共线路；专线的上下行速度一致的，而家庭宽带上行波动偏差很大。另外，专线的价格和家庭宽带价格相差很大，同样100M的，家庭宽带可能130左右一个月，但专线的话估计要上3000左右，两者的价格相差了20倍不止。

Frame Relay（帧中继）是一个比专线经济的配置办法。假设一家公司有两个办公区，最简单的方法就是用Dedicated Line (专线) 把两点连接起来。如果这家公司未来增加办公区，就要再增加两条专线将其接通。

![](https://i.postimg.cc/mkHjZHMj/00-43.png)

但一条专线很贵，如果有十几百来台都直达连接到汇聚路由器，路由器端口本身就很少，这也不现实，显然并非理想的解决办法。帧中继的出现就是让我们可以通过ISP提供的该项服务，与不同位置点建立连线。ISP用Frame Relay Network同时服务多个客户，价格当然比专线便宜，且每个路由器仅需一个接口，两者都可以在预算上省成本。

![](https://i.postimg.cc/YCMBQPm7/14-01.png)

### 帧中继发展的必要条件

* 光纤传输线路的使用
随着光纤传输线路的大量使用，数据传输质量大大提高，光纤传输线路的误码率一般低于 。也就是说在通信链路上很少出现错码，即使偶尔出现的误码也可以由终端处理和纠正。
* 用户终端的智能化
用户终端的智能化（比如计算机的使用），使终端的处理能力大大增强，从而可以把分组交换网中由交换机完成的一些功能（如流量控制、纠错等）能够交给终端去完成。

正由于具备这两个必要条件，使得帧中继交换机可省去纠错控制等功能，从而使其操作简单，即降低了费用又减少了时延，提高了信息传输效率又能保证传输质量。

### 帧中继的逆解析功能（frame-relay inverse-arp）

逆向ARP解析在思科的设备上默认是启动，但是当您已配置帧中继的静态映射，那么逆向ARP解析将自动关闭！该功能用于提供一种动态的将二层DLCI号码映射到三层地址的一种方法。使用手工配置（静态映射），有多少远程端，就要相应个数的远程IP与DLCI映射的指令，这会增加大量的管理开销，并且不能适应帧中继拓扑的变化，而帧中继逆向ARP的产生将解决此类问题。

![](https://i.postimg.cc/QMKSnGyC/150413306.png)

## 帧中继实例

DLCI(data link connection identifier)为数据链路标识，DLCI只有本地意义，它是标识两个相邻结点之间逻辑连接，一条端到端的虚电路可能通过多个节点，相邻节点之间每段虚电路都有自己的DLCI。

虚电路是指位于两台网络层设备为发送和接收数据而建立的逻辑通信路径。虚电路分为两种： 

* Permanent Virtual Circuits
* Switched Virtual Circuits

PVC由网管预先定义，维持路由器恒定连接，载波信号为各用户分配PVC，从而降低开销并提高网络性能；SVC是指通信双方一旦通信会话完成便取消的电路。

![](https://i.postimg.cc/63rR8mC3/05-21.png)

R2配置同理

```
//进入（serial）串口设定IP地址
R1(config)#int s1/1
R1(config-if)#ip add 192.168.10.1 255.255.255.0
//封装帧中继格式来让接口知道使用frame-relay协定
R1(config-if)#enc frame-relay 
//serial restart-delay 连续重启延时，0代表不重启，比如30代表30秒后重启路由器
R1(config-if)#serial restart-delay 0
//在GNS3中配置Frame relay switch发送方端口与DLCI
R1(config-if)#frame-relay interface-dlci 101
//思科默认是关闭接口的，给它开启
R1(config-if)#no sh
```

`show frame-relay map`可以看到IP与DLCI的映射，`do ping 对方的ip地址`查看是否连通。

![](https://i.postimg.cc/4xGBnvCY/9-44.png)

帧中继实际上不支持广播，属于NBMA(Non-Broadcast Multi-Access )网络。所谓的broadcast实际上是伪广播，并不是一次性的向多个PVC发送分组，而是以单播的形式向邻居发送分组的副本。再一测试下ping所在路由的ip地址却不联通。

![](https://i.postimg.cc/LszWG5kb/13-04.png)

实际上是所在路由ip地址与自身的DLCI标识建立映射，`frame-relay map ip 192.168.10.2 202 bro`，加上broadcast关键字才能使用伪广播进行通告。逆向ARP解析在思科的设备上默认是启动，但是当您已配置帧中继的静态映射，那么逆向ARP解析将自动关闭！

![](https://i.postimg.cc/rpRdTdfM/2-02.png)


### 帧中继云

多台帧中继组成的云需要变更为[NNI(网络结点接口)](https://baike.baidu.com/item/NNI/5234091)接口类型传输。网络结点可分为两类：
* 转结点：即支持网络连接性能的结点，通过通信线路转接和传递信息，如终端控制器，集中器等
* 访问结点：信息交换的源结点和目标结点，它起到信源和信宿的作用，如终端、主计算机

![](https://i.postimg.cc/3xQ7CXMc/8-53.png)

路由器模拟帧中继交换机（云）配置

```
//成为帧中继交换机
FRSW1(config)#frame-relay switching
FRSW1(config)#int s1/0   
//帧中继不设置IP是因为他不需要用到IP,他是靠DICI寻找目标不是靠IP寻找
FRSW1(config-if)#no ip add
FRSW1(config-if)#enc frame-relay 
FRSW1(config-if)#serial restart-delay 0
FRSW1(config-if)#frame-relay intf-type dce
FRSW1(config-if)#frame-relay route 101 int s1/0 555
FRSW1(config-if)#no sh
FRSW1(config-if)#
FRSW1(config-if)#int s1/1
FRSW1(config-if)#no ip add
FRSW1(config-if)#enc frame-relay 
FRSW1(config-if)#serial restart-delay 0
//接口类型为NNI，帧中继交换机相连要使用NNI类型接口
FRSW1(config-if)#frame-relay intf-type nni
//将本帧中继上555的DLCI口的信息路由到101的口上,就是将两个口连起来
FRSW1(config-if)#frame-relay route 555 int s1/0 101
FRSW1(config-if)#no sh
```

```
//FRSW2主要起中转作用，所以并未配置frame-relay switching
FRSW2(config)#int s1/0
FRSW2(config-if)#no ip add
FRSW2(config-if)#enc frame-relay 
FRSW2(config-if)#serial restart-delay 0
FRSW2(config-if)#frame-relay intf-type nni
FRSW2(config-if)#frame-relay route 555 interface s1/1 202
FRSW2(config-if)#no sh
FRSW2(config-if)#
FRSW2(config-if)#int s1/1
FRSW2(config-if)#no ip add
FRSW2(config-if)#enc frame-relay 
FRSW2(config-if)#serial restart-delay 0
FRSW2(config-if)#frame-relay intf-type dce
FRSW2(config-if)#frame-relay route 202 int s1/0 555
FRSW2(config-if)#no sh
```


,而且还要记得帧中继是一台交换机,交换机除了方便远程管理外IP没其他作用,所以都不设置.



## 模拟运营商

DTE与DCE的概念与区别：

* DTE(Data Terminal Equipment)数字终端设备，指一般的终端或是计算机、打印机等用户端设备
* DCE(DataCircuit-terminatingEquipment)，数字通信设备，通常指调制解调器，多路复用器或数字设备
* DCE一方提供时钟，DTE不提供时钟，但它依靠DCE提供的时钟工作，比如PC与MODEM的连接

多台帧中继组成的云需要变更为[NNI](https://baike.baidu.com/item/NNI/5234091)接口类型传输。



# n

概念

1.帧中继网络的默认类型是NBMA（非广播多路访问）

2.帧中继网络是一种二层网络技术协议用于交换数据

3.帧中继通过DLCI号标示PVC（DLCI号用于转发数据）

4.帧中继基于PVC技术（虚拟通道）

5.帧中继是面向连接的网络协议（通过LMI）

6.LMI 本地管理接口 （用于控制和判断链路的状态）


## 参考文献

* [知乎-大公司是怎么接入网络服务的？](https://www.zhihu.com/question/318806738)
* [googlebook-电子商务概论](https://books.google.nl/books?hl=zh-CN&id=OuF0DwAAQBAJ&q=frame+relay)
* [张曾科-计算机网络](https://books.google.nl/books?id=gUmThRY3RHEC&pg=PA178&lpg=PA178&dq=%E5%B8%A7%E4%B8%AD%E7%BB%A7%E6%A0%BC%E5%BC%8F+%E5%9B%BE%E8%A7%A3)
* [理解并演示：帧中继的逆向解析功能（frame-relay inverse-arp）](https://blog.51cto.com/7658423/1294309)



