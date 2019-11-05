# STP

* 第一代生成树协议：STP/RSTP
* 支持vlan的第二代生成树协议：PVST/PVST+
* 多实例化的第三代生成树协议：MISTP/MSTP

## STP/RSTP

STP协议就在于通过构造一棵自然树的方法达到裁剪冗余环路的目的，同时实现链路备份和路径最优化。RSTP由IEEE 802.1D-1998标准定义的STP改进而来。

第一点改进：为根端口和指定端口设置了快速切换用的替换端口(Alternate Port)和备份端口(Backup Port)两种角色，当根端口/指定端口失效的情况下，替换端口/备份端口就会无时延地进入转发状态，无需等待两倍Forward Delay时间。

第二点改进：在只连接了两个交换端口的点对点链路中，指定端口只需与下游网桥进行一次握手就可以无时延地进入转发状态。如果是连接了三个以上网桥的共享链路，下游网桥是不会响应上游指定端口发出的握手请求的，只能等待两倍Forward Delay时间进入转发状态。

第三点改进：直接与终端相连而不是把其他网桥相连的端口定义为边缘端口(Edge Port)。边缘端口可以直接进入转发状态，不需要任何延时。由于网桥无法知道端口是否是直接与终端相连，所以需要人工配置

## MISTP/MSTP

多生成树协议MSTP(Multiple Spanning Tree Protocol)是IEEE 802.1s中定义的一种新型多实例化生成树协议。该协议精妙之处在于依据支持或不支持MSTP的交换机给划分成不同的区域，分别称作多生成树(MST)域和单生成树(SST)域。在MST域内部运行多实例化的生成树，在MST域的边缘运行RSTP兼容的内部生成树IST。

MSTP生成树分为4种：

1. CIST（Common Internal Spanning Tree）即公共与内部生成树，CIST由CST和IST组成。
2. CST（Common Spanning Tree）即连接交换网络内所有MST域的一棵生成树。
3. IST（Internal Spanning Tree）即各个MST域内的一棵生成树。
4. SST即运行STP/RSTP的交换设备只能属于一个生成树或者MST域中只有一个交换设备构成的单生成树。

> 参考 [生成树协议（STP PVST CST RSTP MSTP）解析](https://blog.51cto.com/cdlaowang/1758103)
