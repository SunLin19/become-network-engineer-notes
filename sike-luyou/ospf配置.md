# ospf配置

## 实验拓扑

![](https://i.postimg.cc/50Vmgf6x/Snipaste-2019-10-24-22-34-22.png)	

> 拓扑参考自 [基于点到点链路的 OSPF 的初始配置](https://www.cisco.com/c/zh_cn/support/docs/ip/open-shortest-path-first-ospf/13687-15.html?dtid=osscdc000357)

```ios	
Router(config)#interface loopback 0	
Router(config-if)#ip add 3.3.3.3 255.255.255.255	
Router(config-if)#interface s0/1/0	
Router(config-if)#ip add 1.1.1.2 255.255.255.0	
Router(config-if)#no shutdown	
Router(config)#router ospf 1	
Router(config-router)#1.1.1.0 0.0.0.255 area 0	
```	

Router1	

```ios	
Router(config)#interface loopback 0	
Router(config-if)#ip add 2.2.2.2 255.255.255.255	
Router(config-if)#int s0/1/1	
Router(config-if)#ip add 1.1.1.1 255.255.255.0	
Router(config-if)#clock rate 64000	
Router(config-if)#router ospf 1	
Router(config-router)#network 1.1.1.0 0.0.0.255 area 0	
Router(config-if)#no shutdown	
```
> 图表摘自 [51cto-OSPF 多区域原理与配置](https://blog.51cto.com/14157628/2392133)

常用的命令（主要用于排错）有：

![](https://i.postimg.cc/1X3Ct3CH/e8e5.png)


## RID、进程、区域

一台路由器如果要运行OSPF协议，必须存在RID。缺省情况下，路由器系统会从当前接口的IP地址中自动选取一个最大值作为RID。每个OSPF进程的RID要保证在OSPF网络中唯一，否则会导致邻居不能正常建立、路由信息不正确的问题。

路由器支持OSPF多进程，可以根据业务类型划分不同的进程。进程号是本地概念，不影响与其它路由器之间的报文交换。因此，不同的路由器之间，即使进程号不同也可以进行报文交换。

区域是从逻辑上将设备划分为不同的组，每个组用区域号来标识。区域的边界是设备，而不是链路。即一个网段（链路）只能属于一个区域，或者说每个运行OSPF的接口必须指明属于哪一个区域。

> 摘抄自 [曹世宏的博客-OSPF命令行配置](https://cshihong.github.io/2017/10/20/OSPF%E5%91%BD%E4%BB%A4%E8%A1%8C%E9%85%8D%E7%BD%AE/)

## clock rate 64000

要想知道配置为什么时钟频率，首先得要知道什么是DCE、DTE？

* **DCE称DataCircuit-terminatingEquipment，数字通信设备**，例如：路由器。
* **DTE英文全称DataTerminalEquipment，数字终端设备**，指一般的终端或是计算机。

DTE与DCE最为明显的区别：DCE一方提供时钟，DTE不提供时钟，但它依靠DCE提供的时钟工作。btw，路由器既可以作为DCE，也可以作为DTE。


关于`Clock rate 64000`的解释：思科建议64000这个值在它默认的信号类型的线上做设置，而更高的值需要其它类型线缆支持，速率设置超过实际物理线缆所能承受的话，可能会发生丢包等问题。

> 参考自 [cisco-clock rate Usage Guidelines](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/interface/command/ir-cr-book/ir-c2.html#wp3930272930)

改进

https://www.cisco.com/c/zh_cn/support/docs/ip/open-shortest-path-first-ospf/47862-ospfdb3.html


配置

https://jingyan.baidu.com/article/636f38bb4d8ff7d6b84610ba.html

https://www.cisco.com/c/zh_cn/support/docs/ip/open-shortest-path-first-ospf/118879-configure-ospf-00.html?dtid=osscdc000357

https://blog.51cto.com/14157628/2392133

https://blog.51cto.com/yangshufan/1961182



