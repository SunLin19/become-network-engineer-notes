# STP

* 第一代生成树协议：STP/RSTP
* 支持vlan的第二代生成树协议：PVST/PVST+
* 多实例化的第三代生成树协议：MISTP/MSTP

## STP/RSTP

快速生成树协议(RSTP)是一种网络协议，可确保以太网的无环拓扑。

## MISTP/MSTP

MSTP生成树分为4种：

1. CIST（Common Internal Spanning Tree）即公共与内部生成树，CIST由CST和IST组成。
2. CST（Common Spanning Tree）即连接交换网络内所有MST域的一棵生成树。
3. IST（Internal Spanning Tree）即各个MST域内的一棵生成树。
4. SST即运行STP/RSTP的交换设备只能属于一个生成树或者MST域中只有一个交换设备构成的单生成树。
