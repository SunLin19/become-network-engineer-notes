# mstp

Multiple Spanning Tree Protocol (MSTP)又名 802.1s。从传统STP至RSTP是速度的进化。而从RSTP至MSTP则是节省硬件资源和层级式管理的进化。

## rstp存在的问题

消耗硬件资源

Cisco所支缓的RSTP(或STP) 都是Per-VLAN Base的，即是说Spanning Tree Topology数目等於VLAN数目。如果交换机有100个VLAN就要处理100个Spanning Tree Topology，包括发送、接收和运算BPDU，VLAN越多，对整个网络所有交换机的CPU和RAM资源都会造成压力。

缺乏区域管理

