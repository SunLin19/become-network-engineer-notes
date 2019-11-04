
# vtp

首先说明下vlan的配置方式：

* 静态vlan：交换机上的端口以手动分配给vlan
* 动态vlan：使用vmps可以根据连接到交换机端口的源mac地址，动态分配vlan
* 电话vlan：将端口配置到语音模式可以使端口支持连接到端口的IP电话

![](https://i.postimg.cc/Hx6X4dLK/2019-11-04-101116.png)

我们的现在要介绍的vtp就是属于动态vlan划分。VTP（VLAN Trunking Protocol）也被称为虚拟局域网干道协议，是思科私有协议。作用是十几台交换机在企业网中，配置VLAN工作量大，可以使用VTP协议，把一台交换机配置成VTP Server, 其余交换机配置成VTP Client,这样他们可以自动学习到server 上的VLAN 信息。

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

VTP Version 1 及 2 都存在一个缺陷，让网络管理人员闻之色变。问题在於无论 Server Mode 或是 Client Mode，只要收到较自己新的 VLAN 资讯都会更新自己的 VLAN Database，这会造成错误更新的风险。经典的例子是一只在网络上的 Switch 因为某些原因而被关机下线了，然後拿去用作其他用途例如放在测试环境中，经过一番折腾之後，这只 Switch 又放回到网络之中。虽然网络人员已相当醒目地把 Switch 的 Configuration 清掉 (<write erase)，但由於 VTP 资讯不是记录在 Configuration 而是记录 Flash Memory 之中，就算 Configuration 清掉，VTP 设定仍然存在，如果这时 Switch 的 VTP Configuration Revision 较新的话，一插进网络中就会意外地把错误的 VLAN Database 更新到网络的 Switch 之中，造成灾难性後果！在这里会用一个实验说明。

> 摘抄自 [Jan Ho-虚拟区域网络中继协定](https://www.jannet.hk/zh-Hans/post/vlan-trunking-protocol-vtp)
