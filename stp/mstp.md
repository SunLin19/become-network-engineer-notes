# mstp

Multiple Spanning Tree Protocol (MSTP)又名 802.1s。从传统STP至RSTP是速度的进化。而从RSTP至MSTP则是节省硬件资源和层级式管理的进化。

## rstp存在的问题

### 消耗硬件资源

RSTP根据vlan数目而生成等量的生成树拓扑，这些生成树拓扑运作对整个网络的交换机的CPU和RAM资源都会造成很大的负担。

### 缺乏隔离规划管理

RSTP构建的生成树就是一整个的网络拓扑而非区域自治化管理，比不上隔离区域管理的高效与安全性。

## mstp简单实例



