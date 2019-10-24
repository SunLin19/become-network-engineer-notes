写作思路

## clock rate 64000

要想知道配置为什么时钟频率，首先得要知道什么是DCE、DTE？

* **DCE称DataCircuit-terminatingEquipment，数字通信设备**，例如：路由器。
* **DTE英文全称DataTerminalEquipment，数字终端设备**，指一般的终端或是计算机。

DTE与DCE最为明显的区别：DCE一方提供时钟，DTE不提供时钟，但它依靠DCE提供的时钟工作。btw，路由器既可以作为DCE，也可以作为DTE。


关于`Clock rate 64000`的解释：思科建议64000这个值在它默认的信号类型的线上做设置，而更高的值需要其它类型线缆支持，速率设置超过实际物理线缆所能承受的话，可能会发生丢包等问题。

> 参考自 [cisco-clock rate Usage Guidelines](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/interface/command/ir-cr-book/ir-c2.html#wp3930272930)

## 实验拓扑

![](https://i.postimg.cc/50Vmgf6x/Snipaste-2019-10-24-22-34-22.png)

> 拓扑参考自 [基于点到点链路的 OSPF 的初始配置](https://www.cisco.com/c/zh_cn/support/docs/ip/open-shortest-path-first-ospf/13687-15.html?dtid=osscdc000357)

Router0

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
Router(config-if)#clock rate 2000000
Router(config-if)#router ospf 1
Router(config-router)#network 1.1.1.0 0.0.0.255 area 0
Router(config-if)#no shutdown
```



配置

https://jingyan.baidu.com/article/636f38bb4d8ff7d6b84610ba.html

https://www.cisco.com/c/zh_cn/support/docs/ip/open-shortest-path-first-ospf/118879-configure-ospf-00.html?dtid=osscdc000357

https://blog.51cto.com/14157628/2392133

https://blog.51cto.com/yangshufan/1961182



