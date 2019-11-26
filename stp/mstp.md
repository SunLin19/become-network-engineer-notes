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
SW1(config)#int range e0/0-1
SW1(config-if-range)#switchport 
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

## mstp笔记

GNS3中的交换机是用路由器镜像模拟的，因此需要将端口降级为二层交换端口，并手动配置dot1Q协议再进行连通。

```ios
SW1(config)#int range e0/0-1
SW1(config-if-range)#switchport 
SW1(config-if-range)#switchport trunk encapsulation dot1q 
SW1(config-if-range)#switchport mode trunk
```

用路由器模拟交换机，还是有一些问题：

* [三层交换机 no switchport 命令](https://blog.csdn.net/chengxiug/article/details/88902274)

回想起[GNS3用路由器模拟PC的几个误区](https://bbs.hh010.com/thread-457308-1-1.html?_dsign=774775df)

* [spanning-tree的type是指的什么？](https://zhidao.baidu.com/question/562426439.html)

此根据其他参考文章配置出现问题，没有达到预期效果，我认为是工具功能过低部分效果不能完全模拟，**但真正原因是我连生成实例的前提都没有创建怎么可能会出现相应的生成效果**。

todo：

* [ ] 242页 《ccna实验指南》
