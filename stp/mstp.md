# mstp

Multiple Spanning Tree Protocol (MSTP)又名 802.1s。从传统STP至RSTP是速度的进化。而从RSTP至MSTP则是节省硬件资源和层级式管理的进化。

## rstp存在的问题

### 消耗硬件资源

RSTP根据vlan数目而生成等量的生成树拓扑，这些生成树拓扑运作对整个网络的交换机的CPU和RAM资源都会造成很大的负担。

### 缺乏隔离规划管理

RSTP构建的生成树就是一整个的网络拓扑而非区域自治化管理，比不上隔离区域管理的高效与安全性。

### mstp简单实例

![]()

## Region 区域

同一Region内的交换机只会处理相同区域里BPDU信息，从而计算出拓扑。要判断是否身处在同一个 Region，Switch 会比较MSTP配置中的3个参数。name、revision、instance-vlan映射相同才算是同一个区域。 。注意MSTP预设instance 0是默认实例用作CIST，不可用作Region。缺省时所有vlan都映射到该实例上。

## mstp笔记

GNS3中的交换机是用路由器镜像模拟的，因此需要将端口降级为二层交换端口，并手动配置dot1Q协议再进行连通。

```ios
SW1(config)#int range e0/0-1
SW1(config-if-range)#switchport 
SW1(config-if-range)#switchport trunk encapsulation dot1q 
SW1(config-if-range)#switchport mode trunk


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
