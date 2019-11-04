
# vtp

首先说明下vlan的配置方式：

* 静态vlan：交换机上的端口以手动分配给vlan
* 动态vlan：使用vmps可以根据连接到交换机端口的源mac地址，动态分配vlan，现在要介绍的vtp就是属于此
* 电话vlan：将端口配置到语音模式可以使端口支持连接到端口的IP电话

![](https://i.postimg.cc/Hx6X4dLK/2019-11-04-101116.png)

VTP（VLAN Trunking Protocol）也被称为虚拟局域网干道协议，是思科私有协议。作用是十几台交换机在企业网中，配置VLAN工作量大，可以使用VTP协议，把一台交换机配置成VTP Server, 其余交换机配置成VTP Client,这样他们可以自动学习到server 上的VLAN 信息。

> 摘抄自 [百度百科-vtp](https://baike.baidu.com/item/VTP)

## vtp模式

推荐结合[Jan Ho-虚拟区域网络中继协定](https://www.jannet.hk/zh-Hans/post/vlan-trunking-protocol-vtp)观看

![](https://i.postimg.cc/m2C3Kxpc/2019-11-04-101337.png)

> 以上两图截选自[红茶三杯-交换基础 VLAN TRUNK VTP](http://v.youku.com/v_show/id_XMzY4ODQwNDI4.html)

### vtp示例

由于交换机默认为server模式，S1这里就未做重复配置了。

```ios
S1(config)#vtp domain net3141
S1(config)#int f0/1
S1(config-if)#switchport mode trunk 
```
配置S2客户端

```ios
//VLAN Database保持同步，必须Server与Client的Domain要相同
S2(config)#vtp domain net3141
S2(config)#vtp mode client 
S2(config)#int f0/1
S2(config-if)#switchport mode trunk 
```
在server中(S1)创建vlan 10、20、30后，在到client(S2)使用`show vtp staus`查看vtp状态信息

![](https://i.postimg.cc/XNBSZk7K/40-58.png)

其中Configuration Revision是VLAN Database的版本流水号，每次Server的VLAN Database有更新，此流水号就会加1。

### V3

VTP Version1及2都存在一个缺陷，让网络管理人员闻之色变。问题在於无论Server Mode或是 Client Mode，只要收到较自己新的VLAN资讯都会更新自己的VLAN Database，这会造成错误更新的风险。经典的例子是一只在网络上的Switch因为某些原因而被关机下线了，然後拿去用作其他用途例如放在测试环境中，经过一番折腾之後，这只Switch又放回到网络之中。虽然网络人员已相当醒目地把Switch的Configuration清掉 (write erase)，但由於 VTP 资讯不是记录在 Configuration 而是记录Flash Memory之中，就算Configuration清掉，VTP设定仍然存在，如果这时Switch的VTP Configuration Revision较新的话，一插进网络中就会意外地把错误的 VLAN Database更新到网络的Switch之中，造成灾难性後果！

> 摘抄自 [Jan Ho-虚拟区域网络中继协定](https://www.jannet.hk/zh-Hans/post/vlan-trunking-protocol-vtp)

使用`vtp version 3`即可升级协议，因为我们必需把Server升级成Primary Server才有权更改VLAN，升级的指令是`vtp primary vlan`，若Primary ID与本机 Device ID一致，则代表本机正是Primary Server。

## vtp pruning

引入VTP修剪功能的出发点是为了限制对于邻居交换机来说没有意义的广播、组播和未知单播流量在中继链路上传输，以节省中继链路的带宽使用。VTP修剪功能会自动根据邻居交换机是否有成员端口分配到该VLAN来决定是否向此邻居交换机发送该VLAN中的流量，以实现对某些VLAN中的泛洪流量进行修剪。

在实际配置中，管理员还可针对具体的Trunk端口（可以在VTP客户交换机上，但更多是在VTP服务器上）进行VTP修剪资格列表配置，手工指定允许进行修剪的VLAN，即修剪资格列表。

VTP修剪很简单，只要在VTP Server用`vtp pruning`即可，整个VTP域里的交换机都会启用VTP Pruning模式。
