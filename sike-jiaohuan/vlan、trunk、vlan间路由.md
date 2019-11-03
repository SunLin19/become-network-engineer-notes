# vlan、trunk、vlan间路由

## vlan

虚拟局域网（VLAN）是一组逻辑上的设备和用户，这些设备和用户并不受物理位置的限制，可以根据功能、部门及应用等因素将它们组织起来，相互之间的通信就好像它们在同一个网段中一样。工作在OSI参考模型的第2层和第3层，一个VLAN就是一个广播域，VLAN之间的通信是通过第3层的路由器来完成的。与传统的局域网技术相比较，VLAN技术更加灵活，它具有以下优点： 
* 网络设备的移动、添加和修改的管理开销减少
* 可以控制广播活动
* 可提高网络的安全性

> 摘抄 [百度百科-虚拟局域网](https://baike.baidu.com/item/%E8%99%9A%E6%8B%9F%E5%B1%80%E5%9F%9F%E7%BD%91/419962?fromtitle=VLAN)

大多数企业同一子网内的网络也都会把不同部门人员传输访问进行隔离，确保部门分工明确的运作及资料保密性提高，又可以把故障和错误控制在更小的范围内。

![](https://i.postimg.cc/qRQCHSJW/57-31.png)

## trunk

trunk有两种含义，一种trunk端口汇聚的，就是把几个物理端口汇聚成一个更大带宽的逻辑端口，达到增加带宽的目的。另一种trunk是VLAN trunk，主要是为了承载多交换机多个vlan间的相互通信；**若是跨vlan的通信必须通过三层设备来实现网络通信，也就是vlan间路由，而非trunk。** 如：公司大楼每层的部门及分布在不同楼层的隶属办公部门，这时就需要确保多台交换机处于同一通信。

> 参考 [百度知道-为什么要设置trunk口？](https://zhidao.baidu.com/question/276567832.html)

建立trunk链路也是需要标准才能进行通信 —— IEEE 802.1q协议。

### dot1q

dot1q就是802.1q，dot就是点的意思，就简写为dot1q了。**IEEE 802.1q以及VLAN Tagging属于互联网下IEEE 802.1的标准规范**，允许多个网桥(Bridge)在信息不被外泄的情况下公开的共享同一个实体网上。IEEE 802.1q-英文缩写写为dot1q，经常在实现以太网封装协议的架构下被提及。

IEEE 802.1q定义一个关于VLAN连接介质访问控制层和IEEE 802.1D生成树协议的具体概念模型。**这个模型允许各个独立的VLAN与以太网交换机的数据链路层或路由器互相连接。**

> 摘抄自 [百度百科-802.1q](https://baike.baidu.com/item/802.1q)

关于dot1q协议具体的运作实现方式请看：[OceanF-VLAN IEEE802.1Q](https://www.cnblogs.com/OceanF/p/9230299.html)

### 配置实验

![](https://i.postimg.cc/LsJ4HWvw/21-46.png)

首先将路由器模拟成一台PC，R2、R3也是如此。

```ios
//关闭路由功能，用于模拟pc
R1(config)#no ip routing
R1(config)#int f0/0
R1(config-if)#ip add 192.168.1.1 255.255.255.0
R1(config-if)#no shutdown
//不涉及不同的CIDR互访，默认网关可配可不配
R1(config)#ip default-gateway 192.168.1.254
```

在交换机将PC地址划分置相应vlan并分配给交换机相应端口，其他SW同理。


```ios
SW1(config)#vlan 10
SW1(config-vlan)#int f0/1
// 在f0/1口内配置二层接入接口访问模式
SW1(config-if)#switchport mode access 
//指定专属的vlan编号
SW1(config-if)#switchport access vlan 10
```
在双方交换机上共同配置trunk模式。

```ios
//2950只支持dot1q，所以不用写switchport trunk encapsulation dot1q/isl命令
//3560以上才支持dot1q和isl，才有encapsulation的命令
SW1(config)#int f0/3
SW1(config-if)#switchport mode trunk
```

## vlan间路由 

**在配置实验基础之上进行跨vlan通信**。单臂路由通过一条物理链路实现VLAN间路由，但Trunk链路需选择带宽较高的链路。最好使用三层交换机实现VLAN划分、VLAN内部二层交换和VLAN间路由的功能。单臂路由造成的延迟、带宽和路由器运算压力，使用三层交换机可以得到解决。

![](https://i.postimg.cc/wMJXvgjs/27-05.png)

在交换机将PC地址划分置相应vlan并分配给交换机相应端口，在与路由交接的链路端口开启trunk模式。

```ios
Switch(config-if)#vlan 2
Switch(config-vlan)#int f0/2
Switch(config-if)#switchport access vlan 2
Switch(config-if)#exit
Switch(config)#int f0/3

Switch(config-if)#vlan 3
Switch(config-vlan)#int f0/3
Switch(config-if)#switchport access vlan 3
Switch(config-if)#exit

Switch(config)#int f0/1
Switch(config-if)#switchport mode trunk 
```
路由器开启接口，配置通信协议，添加相应IP地址即可。

```ios
Router(config)#int fa0/0
Router(config-if)#no shutdown 
Router(config-if)#exit

//子接口 f0/x.y为开启x的编号y的子接口
Router(config)#int fa0/0.1
//配置vlan的trunk链路的通信协议
Router(config-subif)#encapsulation dot1Q 2
Router(config-subif)#ip address 192.168.1.1 255.255.255.0
Router(config-subif)#exit

Router(config)#int fa0/0.2
Router(config-subif)#encapsulation dot1q 3
Router(config-subif)#ip address 192.168.2.1 255.255.255.0
Router(config-subif)#end
```

### 笔记

回顾 [静态路由](sike-luyou/静态路由.md)，加深印象以及`ip route`配置命令的理解；需要用到的辅助查看命令：

* `show ip route` 查看IP路由
* `show ip interface brief` 查看端口IP及开启状态
* `show vlan brief ` 查看配置vlan的端口状态
