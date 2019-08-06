# 简说arp协议

## arp协议概述

IP数据包常通过以太网发送。以太网设备并不识别32位IP地址：它们是以48位以太网地址传输以太网数据包的。因此，IP驱动器必须把IP目的地址转换成以太网网目的地址。在这两种地址之间存在着某种静态的或算法的映射，常常需要查看一张表。地址解析协议(Address Resolution Protocol，ARP)就是用来确定这些映象的协议。

> 摘自 [IBM - ARP协议揭密](https://www.ibm.com/developerworks/cn/linux/l-arp/index.html)

## arp工作机制

(主机A)192.168.0.1与192.168.0.3(主机B)通信

