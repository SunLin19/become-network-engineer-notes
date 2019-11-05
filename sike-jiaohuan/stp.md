# STP

* 第一代生成树协议：STP/RSTP
* 支持vlan的第二代生成树协议：PVST/PVST+
* 多实例化的第三代生成树协议：MISTP/MSTP

## STP/RSTP

STP协议就在于通过构造一棵自然树的方法达到裁剪冗余环路的目的，同时实现链路备份和路径最优化。RSTP由IEEE 802.1D-1998标准定义的STP改进而来。

## MISTP/MSTP

多生成树协议MSTP(Multiple Spanning Tree Protocol)是IEEE 802.1s中定义的一种新型多实例化生成树协议。该协议精妙之处在于依据支持或不支持MSTP的交换机给划分成不同的区域，分别称作多生成树(MST)域和单生成树(SST)域。在MST域内部运行多实例化的生成树，在MST域的边缘运行RSTP兼容的内部生成树IST。

MSTP生成树分为4种：

1. CIST（Common Internal Spanning Tree）即公共与内部生成树，CIST由CST和IST组成。
2. CST（Common Spanning Tree）即连接交换网络内所有MST域的一棵生成树。
3. IST（Internal Spanning Tree）即各个MST域内的一棵生成树。
4. SST即运行STP/RSTP的交换设备只能属于一个生成树或者MST域中只有一个交换设备构成的单生成树。

> 参考 [生成树协议（STP PVST CST RSTP MSTP）解析](https://blog.51cto.com/cdlaowang/1758103)
