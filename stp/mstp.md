# mstp

Multiple Spanning Tree Protocol (MSTP)又名 802.1s。从传统STP至RSTP是速度的进化。而从RSTP至MSTP则是节省硬件资源和层级式管理的进化。

## rstp存在的问题

### 消耗硬件资源

RSTP根据vlan数目而生成等量的生成树拓扑，这些生成树拓扑运作对整个网络的交换机的CPU和RAM资源都会造成很大的负担。

### 缺乏隔离规划管理

RSTP构建的生成树就是一整个的网络拓扑而非区域自治化管理，比不上隔离区域管理的高效与安全性。

## mstp简单实例

![](https://i.postimg.cc/Pr2Z1v8w/Snipaste-2019-11-26-23-05-37.png)

### Region 区域

MSTP中各个区域的生成树互不影响各自的网络拓扑结构。同一Region内的交换机只会处理相同区域里BPDU信息，从而计算出拓扑。要判断是否身处在同一个Region，交换机会比较MSTP配置中的3个参数。name、revision、instance-vlan映射相同才算是同一个区域。注意MSTP预设instance 0是默认实例用作CIST，不可用作Region。缺省时所有vlan都映射到该实例上。

### 配置

**GNS3中的交换机是用路由器镜像模拟的，因此需要将端口降级为二层交换端口，并手动配置dot1Q协议再进行连通。** 以SW1为例：

```ios
//多个端口共同配置参数
SW1(config)#int range e0/0-1
//将三层端口降级成二级端口
SW1(config-if-range)#switchport 
//配置trunk互通协议
SW1(config-if-range)#switchport trunk encapsulation dot1q 
SW1(config-if-range)#switchport mode trunk
```

创建vlan2~4，以及各个交换机配置trunk模式命令重合，这里就仅做篇幅省略。以下结合上方拓扑对各个交换机的配置

```
SW1(config)#sp mo mst
SW1(config)#sp mst conf
SW1(config-mst)#name cisco
SW1(config-mst)#revision 1
SW1(config-mst)#ins 1 vl 1-2
SW1(config-mst)#ins 2 vl 3-4
SW1(config-mst)#exit
SW1(config)#sp mst 0 pri 4096
SW1(config)#sp mst 1 pri 4096
SW1(config)#sp mst 2 pri 8192
```

```
SW2(config)#sp mo mst
SW2(config)#sp mst conf
SW2(config-mst)#name cisco
SW2(config-mst)#re 1
SW2(config-mst)#ins 1 vl 1-2
SW2(config-mst)#ins 2 vl 3-4
SW2(config-mst)#exit
SW2(config)#sp mst 1 pri 8192
SW2(config)#sp mst 2 pri 4096
```

```
SW3(config)#sp mo ms
SW3(config)#sp ms c
SW3(config-mst)#name cisco
SW3(config-mst)#re 1
SW3(config-mst)#ins 1 vl 1-2
SW3(config-mst)#ins 2 vl 3-4
SW3(config-mst)#exit
SW3(config)#do sh sp ms
```

## VTP v3配合MSTP

![](https://i.postimg.cc/vZR5rZHf/04-51.png)

如果我们有百来台大量的交换机，以及百来个过多的vlan需要配置，显然在交换机一个个去添加是很不现实的，，因此，就需要结合前面所学的[虚拟局域网干道协议（VTP）](sike-jiaohuan/vtp.md)配合MSTP相互协作来进行组网。

以下，结合之前的MSTP拓扑进行VTP v3，用VTP发放MSTP有以下几点需要注意的：

* 由于每一个MST Region的设定是不相同的，因此每个区域都需要放于不同的VTP Domain之中
* region、revision、instance-vlan（key-value）均等同才算同一个MST Region

为了不引起混乱，在整个网络中只设置一个server。如果网络中有新的交换机加入的话，最好也吧它改为非server模式，防止vtp混乱！

参考示例：[VTP Version 3 and MSTP Walkthrough.](http://sabotage-networks.blogspot.com/2010/02/vtp-version-3-and-mstp.html)、[jan ho - 多重生成树协定](https://www.jannet.hk/zh-Hans/post/multiple-spanning-tree-protocol-mstp/)

## mstp笔记

### base

用路由器模拟交换机，还是有一些问题：三层端口不能使用二层端口配置

解决：查看了[三层交换机 no switchport 命令](https://blog.csdn.net/chengxiug/article/details/88902274)，降级并手动配置dot1Q协议再进行连通。

```ios
SW1(config)#int range e0/0-1
SW1(config-if-range)#switchport 
SW1(config-if-range)#switchport trunk encapsulation dot1q 
SW1(config-if-range)#switchport mode trunk
```
配置生成树实例没有出现预期效果，我认为是工具功能过低部分效果不能完全模拟，**但真正原因是mst instance基于vlan而形成的，我连vlan都没有创建怎么可能出现相应实例**。

### vtp v3

由于有段时间没复习vtp，印象不深。到底是先设定mstp还是vtp，犯了难；但经过一次实验，发现vtp会影响整个网络所有配置了trunk模式的交换机，所以先设定mstp，再配置vtp，防止vtp破坏mstp的隔离。？

进行创建及自动下发vlan时，需升级成主服务器并在配置时，`do vtp primary`。



