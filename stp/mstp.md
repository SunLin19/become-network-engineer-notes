# mstp

Multiple Spanning Tree Protocol (MSTP)又名 802.1s。从传统STP至RSTP是速度的进化。而从RSTP至MSTP则是节省硬件资源和层级式管理的进化。

## rstp存在的问题

### 消耗硬件资源

Cisco所支持的RSTP(或STP)都是Per-VLAN Base的，即是说Spanning Tree Topology数目等於VLAN数目。如果交换机有100个VLAN就要处理100个Spanning Tree Topology，包括发送、接收和运算BPDU，VLAN越多，对整个网络所有交换机的CPU和RAM资源都会造成压力。

MSTP提出了Instance的想法，把多个VLAN分组成为不同的Instance，同一组Instance的VLAN共用一组Spanning Tree Topology。举例：VLAN101至VLAN200共 100组VLAN，可把VLAN101至VLAN150编入Instance 1，馀下的VLAN151至VLAN200编入Instance 2。Spanning Tree Topology 数量由100个大大缩减至2个。

### 缺乏区域管理

RSTP缺乏区域概念，难於管理。MSTP可把整个网络分区(Region)管理，Region与Region之间不会互相干扰Topology。每个Region会构成一只巨大的虚拟交换机(Virtual Switch)，每个Virtual Switch之间会进行一个独立的Spanning Tree运算，而这个称为Common and Internal Spanning Tree (CIST)又不会影响 Region里的Topology，听起来相当複杂，在之後的部份将会深入说明。
