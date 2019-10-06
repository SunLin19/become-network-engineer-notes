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

### 实验拓扑

![](https://i.postimg.cc/8PTthcGc/214718.png)

### 桌面及路由配置

PC0桌面端网络配置，PC1也是同理

```
IP Address: 192.168.10.1
Sub Mark: 255.255.255.0
Default Gateway: 192.168.10.254
```

路由器Router0（简称R0）配置

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

