# pvst生成树实例

## PVST+选举

### 选举说明

PVST+ 是Cisco交换机预设的STP，通过PVST+，交换机可以建立一个无环路的拓扑。各交换会按程序完成以下步骤：

1. 在整个网络里选举一个Root Switch（Switch Priority或称Bridge Priority）
2. 除了 Root Switch 外的其他各 Switch 都选择一个 Root Port
3. 所有网段选择一个 Designated Port
4. 把没成为Root Port或Designated Port的端口成为Non-Desiganted Port，这些端口会被禁用，以去防止环路发生

交换机间各端口的选举规则:

若规则相比较数值对等，则由顶向下层层对比进行选举

* 规则1：比 Bridge ID，小的胜出 (只有选 Root Switch 时使用)
* 规则2：比 Root Cost，小的胜出
* 规则3：比对方的 Bridge ID，小的胜出
* 规则4：比对方的 Port ID，小的胜出

### 拓扑展示

我们先将各个交换机端口置为trunk模式进行互通，以便直观展示生成树实例，拓扑如下：

![](https://i.postimg.cc/ZKC1Nyj2/Snipaste-2019-11-08-20-57-25.png)

### 选举BP（Bridge Priority）

首先在拓扑中选举Root Switch，由Bridge ID小的胜出，Bridge ID即优先级和交换机mac地址共同构成的一组数值。默认优先级等同情况下再比较mac地址选出Root Switch。还有个显著的特征根网桥端口全为指定端口。

* Switch Priority = 32768(默认优先值)+vlan id
* 图中bridge id处的address即为交换机的mac地址
* 默认情况下，所有交换机端口都隶属vlan 1

![](https://i.postimg.cc/Y9xB9bpS/11-02.png)

可通过修改交换机vlan1的优先级，让其他交换机成为BP（Root Switch），注意更改优先级必须是4096整倍数。

```
//当前交换机修改vlan id的优先级后的数值比其他交换机数值低后，
//那么它就会成为该vlan id的根网桥
spanning-tree vlan [id] priority [num]
```

![](https://i.postimg.cc/Sxk9rGLR/45-46.png)


### 选举RP（Root Port）

除Bridge Priority胜出的Root Switch，其他交换机都会选择一个RP，根端口是最接近Root Switch的端口，“最近”按网口带宽的开销值大小比较。

![](https://i.postimg.cc/qMQjgCBC/49-17.png)

SW3用f0/1经过SW1需1条100Mb的链路，而用f0/2要经过2条(Cost=19+19=38)，因此SW2选用f0/1为RP根端口，SW2同理。

![](https://i.postimg.cc/RhcPdw1y/40-28.png)

当然，我们可以进入f0/1接口用`spanning-tree vlan [id] cost [number]`来增加cost，致使f0/2成为RP。

![](https://i.postimg.cc/4N6Qnz1H/Snipaste-2019-11-09-23-02-08.png)

### 选举DP(Designated Port)与阻塞非指定端口

在每组相连交换机端口选一个作为指定端口，先比较链路开销cost，如果开销相同，则后比较bridge id（priority+mac）。剩下落选的端口，也就是非指派端口(Non-Designated Port)，作为Root Port的备份端口，以提供该交换机从其他交换机的DP到根桥的另一条可切换路径。

注：两个交换机相连为一组，没有网段一词的原因，参考[百度百科-网段](https://baike.baidu.com/item/%E7%BD%91%E6%AE%B5)

![](https://i.postimg.cc/1590mFw3/a19.png)


  ## 桥协议数据单元

Bridge Protocol Data Unit (BPDU) 是交换机间用来传送STP信息的帧，BPDU包含所有STP选举所需要的信息。一个网络中应该只有Root Switch发放BPDU，不过当任何一台交换机刚启动时，它都会认为自己是Root Switch而发放BPDU，直至它收到一个包含Bridge ID较小的BPDU (称为 Superior BPDU) 时，它才知道自己在Root Switch选举中落败了而停止发放BPDU。

BPDU预设每2秒发放一次，称为BPDU Hello Time，而交换机接收到的BPDU只会被储存20秒 (Max-age)，即是说如果交换机过了20秒没有收BPDU，也没有在其他端口收到BPDU的话，就会判断Root Switch已经死了，它又会再次认定自己是RS，再次发放BPDU了。STP还有另一个Timer（定时器）称为Forward Delay，是用作调校Listening和Learning状态的时间。

由于更改STP的Timer有一定风险，如果设定值不理想会导致整个网络瘫痪，因此不建议更改，就算要更改都请尽量使用Diameter指令，这样可以使用Cisco的建议值，对照表如下：

![](https://i.postimg.cc/59gcTFmQ/56-10.png)

## PVST+端口状态

在STP中，一个Port由Down变成Up，中间经历了几个状态，了解这些状态是学习STP重要的一环。

![](https://i.postimg.cc/PxrsKmR4/6-03.png)


阻断状态不会传送或接收数据帧，但仍会接收BPDU，目的是Blocking Port有需要知道时时生成树的状态，当有需要时可以跳到Listening。为了防止环路发生，一台交换机启动时，所有端口都会先进入Blocking。

当交换机知道某个端口需要启动时，就会把端口由Blocking转到Listening，在这个状态下端口会传送及接收BPDU，但仍不会传送数据帧，它会参与Root Switch 选举、Root Port选举和Designated Port 选举。这个状态会维持一个Forward Delay的时间 (预设15秒)。

如果一个端口在选举时成功成为RP或DP，它便会进入Learning，端口会收发BPDU，而且会开始留意传过来的数据帧，把MAC地址记录到MAC-Address Table，但仍然不会传送数据帧，这样做的目的是希望这个端口在开始工作之前，先记录MAC地址以尽量避免泛洪所有端口产生不必要的流量。这个状态会维持一个Forward Delay的时间 (预设15秒)。

![](https://i.postimg.cc/65THcH5q/43569.jpg)

## 拓扑变更

### PortFast

这个是用于接入口的，一个正常的交换接口从down到up要经过:Down、listening、learning、fowarding几个状态，一共耗时为30秒，从而决定此端口是blocking还是fowarding的，也是交换机的防止环路的机制。但是对于直接接入PC这样的终端设备的接口就没有必要经过这几步了，也就是从down直接进入fowarding的状态。基本的配置方法：

```
Sw(config)#spanning-tree portfast default// （所有接口启用）全局下用
Sw(config-if)#spanning-tree portfast //接口下单独启用
Sw(config-if)#spanning-tree portfast disable //某个口禁用，通常是连接另一台交换机的口
```
> 摘抄[百度知道-Cisco 交换机 spaning-tree portfast 什么意思？](https://zhidao.baidu.com/question/170494428.html)

### UplinkFast

当某台交换机Root Port与Bridge Priority连接中断造成***Directly Fail***，此时该交换机的Alternate Port由blocking转到forwarding也需要30秒时间，这个对于一个网络来说可谓完全不能接受，启动了UplinkFast的交换机会选1个 Blocking Port设为Standby，当Root Port死掉时，就绪的端口立刻转成Forwarding，省掉30秒等候时间。

**开启UplinkFast的指令为`spanning-tree uplinkfast`,请注意，此指令不应在Root Switch执行，优先级及开销数值也会发生变化。**

### BackboneFast

Directly Fail 可用 UplinkFast 解决，现在讨论一下 Indirectly Fail 会出现什麽状况。

## 参考链接

https://www.cisco.com/c/en/us/td/docs/routers/access/3200/software/wireless/SpanningTree.html?


* [Jan Ho-Spanning Tree Protocol (STP) 生成树协定](https://www.jannet.hk/zh-Hans/post/spanning-tree-protocol-stp/#stp)
* [大S博客-STP生成树协议之根端口/指定端口的手工选举](http://www.slyar.com/blog/stp-port-election.html)
