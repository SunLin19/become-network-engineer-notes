# rip协议配置

在配置rip协议之前，首先知道要知道几个点
* rip协议是什么？
* rip有什么作用？
* 它有哪些优点，以及不足之处？

## 概览rip协议

RIP即路由信息协议，是一种分布式的基于距离<ins>向量</ins><sup>①</sup>的<ins>路由</ins><sup>②</sup>选择协议

注：

① 同时具有大小和方向的量，与向量相对的是只有大小没有方向的标量  
② 路由就是为发送方找到去往目的地址的路径的过程

**rip协议行为协定：**

在网络中每一个路由器都要维护从它自己到其他每一个目的网络的距离记录（这一组距离即是距离向量）

**rip关于距离的定义：**

* 从路由器到直连到网络设备的距离为1；从源路由器到N个路由器的距离（非直连）为N；该距离也被称为跳数
* 另外，rip允许最多只包含15个路由器，16跳数为不可达，可见只适用于小型互联网

**rip协议的作用：**

不配置rip，就只能学到自己的直连路由，举个例子

```
A---B---C---D
```

若不配配置rip，A只知道AB之间的路由，BC之间的它不知道，CD之间的它也不知道；如果有个pc连到A上，这个pc要访问CD之间的网络，它就会把报文交给A，但是A的路由表中没有相应的路由，所以就会丢弃，这样PC就无法访问。

配置rip就可以让路由器动态地学习在一定范围内的所有的路由信息，当然也可以手工配置静态路由，但是这样的工作量比较大，而且也容易出错。

## rip配置实验

实验拓扑

![](https://i.postimg.cc/8PTthcGc/214718.png)

PC0桌面端网络配置，PC1也是同理

```
IP Address: 192.168.10.1
Sub Mark: 255.255.255.0
Default Gateway: 192.168.10.254
```

路由器Router0（简称R0）配置；另外，可使用`debug ip rip`观察rip更新过程，`show ip route`查看路由表

```ios
Router>en // 进入特权模式
Router#conf t // 进入全局配置模式
Router(config)#int f0/0 // 进入快速以太网端口 f0/0
Router(config-if)#ip add 192.168.10.254 255.255.255.0 // 配置网关
Router(config-if)#no shutdown // 开启接口
Router(config-if)#int f0/1 // 进入快速以太网端口 f0/1
Router(config-if)#ip add 192.168.20.1 255.255.255.0 // 配置网关
Router(config-if)#no shutdown // 开启接口
Router(config-if)#exit   // 退出到全局模式
Router(config)#router rip // 开启 rip 协议
Router(config-router)#network 192.168.10.0 // 宣告直连网段
Router(config-router)#network 192.168.20.0 // 宣告直连网段
```

<details><summary>路由器R1配置</summary>

```ios
Router>en // 进入特权模式
Router#conf t // 进入全局配置模式
Router(config)#int f0/0 // 进入快速以太网端口 f0/0
Router(config-if)#ip add 192.168.20.2 255.255.255.0 // 配置网关
Router(config-if)#no shutdown // 开启接口
Router(config-if)#int f0/1 // 进入快速以太网端口 f0/1
Router(config-if)#ip add 192.168.30.1 255.255.255.0 // 配置网关
Router(config-if)#no shutdown // 开启接口
Router(config-if)#exit   // 退出到全局模式
Router(config)#router rip // 开启 rip 协议
Router(config-router)#network 192.168.20.0 // 宣告直连网段
Router(config-router)#network 192.168.30.0 // 宣告直连网段
```

</details>


<details><summary>路由器R2配置</summary>

```ios
Router>en // 进入特权模式
Router#conf t // 进入全局配置模式
Router(config)#int f0/0 // 进入快速以太网端口 f0/0
Router(config-if)#ip add 192.168.30.2 255.255.255.0 // 配置网关
Router(config-if)#no shutdown // 开启接口
Router(config-if)#int f0/1 // 进入快速以太网端口 f0/1
Router(config-if)#ip add 192.168.40.254 255.255.255.0 // 配置网关
Router(config-if)#no shutdown // 开启接口
Router(config-if)#exit   // 退出到全局模式
Router(config)#router rip // 开启 rip 协议
Router(config-router)#network 192.168.30.0 // 宣告直连网段
Router(config-router)#network 192.168.40.0 // 宣告直连网段
```

</details>

R1、R2的配置也是如此，也是将彼此相邻的网关、网段进行配置通告。

## RIP工作原理

**通过实验总结rip工作原理，会发现之前关于rip的定义概述，一下子就清晰了许多。相关概述及优缺点都离不开运行原理，可以说是对原理进一步的总结或是一种叙述。**

1. 路由建立

   路由器运行RIP后，会首先发送路由更新请求，收到请求的路由器会发送自己的RIP路由进行响应；网络稳定后，路由器会周期性发送路由更新信息。

2. 距离矢量的计算

   RIP度量的单位是跳数，其单位是1，也就是规定每一条链路的成本为1，而不考虑链路的实际带宽、时延等因素，RIP最多允许15跳。

   RIP利用度量来表示它和所有已知目的地间的距离。

   当一个RIP更新报文到达时，接收方路由器和自己的RIP路由表中的每一项进行比较，并按照距离矢量路由算法对自己的RIP路由表进行修正。

3. 定时器

    周期更新定时器：用来激发RIP路由器路由表的更新，每个RIP节点只有一个更新定时器，设为30s。每隔30s路由器会向其邻居广播自己的路由表信息。每个RIP路由器的定时器都独立于网络中其他路由器，因此它们同时广播的可能性很小。

    超时定时器：用来判定某条路由是否可用。每条路由有一个超时定时器，设为180s。当一条路由激活或更新时，该定时器初始化，如果在180s之内没有收到关于那条路由的更新，则将该路由置为无效。

   清除定时器：用来判定是否清除一条路由。每条路由有一个清除定时器，设为120s。当路由器认识到某条路由无效时，就初始化一个清除定时器，如果在120s内还没收到这条路由的更新，就从路由表中将该路由删除。

    延迟定时器：为避免触发更新引起广播风暴而设置的一个随机的延迟定时器，延迟时间为1～5s。

4. 环路

   当网络发生故障时，RIP网络有可能产生路由环路。可以通过<ins>水平分割、毒性反转</ins>、触发更新、抑制时间等技术来避免环路的产生。

> 参考自 [一个动画看懂网络原理之RIP协议的路由表的建立过程（网络篇）](http://baijiahao.baidu.com/s?id=1600198379976045804&wfr=spider&for=pc)

![](https://i.postimg.cc/HnGDp8VT/7b2cf.gif)

## 学习笔记

### 名词疑难点

水平分割指的是什么？什么是毒性反转？

所找到的参考资料：[cshihong-RIP基础知识](https://cshihong.github.io/2018/03/23/RIP%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/#%E6%AF%92%E6%80%A7%E5%8F%8D%E8%BD%AC%EF%BC%9A)

### 命令配置问题

特权模式`enable`、全局配置模式`conf t`存在的意义？

为什么每次为端口分配ip后都需要需要no shutdown？

No shutdown是开启端口的意思,路由器启机以后端口默认的状态是shutdown的,所以必须用no shutdown来开启端口


