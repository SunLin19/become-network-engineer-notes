# mstp

Multiple Spanning Tree Protocol (MSTP)又名 802.1s。从传统STP至RSTP是速度的进化。而从RSTP至MSTP则是节省硬件资源和层级式管理的进化。

## rstp存在的问题

### 消耗硬件资源

RSTP根据vlan数目而生成等量的生成树拓扑，这些生成树拓扑运作对整个网络的交换机的CPU和RAM资源都会造成很大的负担。

### 缺乏隔离规划管理

RSTP构建的生成树就是一整个的网络拓扑而非区域自治化管理，比不上隔离区域管理的高效与安全性。

## mstp简单实例

GNS3中的交换机是用路由器镜像模拟的，因此需要将端口降级为二层交换端口，并手动配置dot1Q协议再进行连通。

![]()

```ios
SW1(config)#int range e0/0-1
SW1(config-if-range)#switchport 
SW1(config-if-range)#switchport trunk encapsulation dot1q 
SW1(config-if-range)#switchport mode trunk
```


## mstp笔记

用路由器模拟交换机，还是有一些问题：

* [三层交换机 no switchport 命令](https://blog.csdn.net/chengxiug/article/details/88902274)

回想起[GNS3用路由器模拟PC的几个误区](https://bbs.hh010.com/thread-457308-1-1.html?_dsign=774775df)

