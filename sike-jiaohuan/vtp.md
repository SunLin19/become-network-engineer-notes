
写作思路

## vtp

首先说明下vlan的配置方式：

* 静态vlan：交换机上的端口以手动分配给vlan
* 动态vlan：使用vmps可以根据连接到交换机端口的源mac地址，动态分配vlan
* 电话vlan：将端口配置到语音模式可以使端口支持连接到端口的IP电话

![](https://i.postimg.cc/Hx6X4dLK/2019-11-04-101116.png)

我们的现在要介绍的vtp就是属于动态vlan划分。VTP（VLAN Trunking Protocol）也被称为虚拟局域网干道协议，是思科私有协议。作用是十几台交换机在企业网中，配置VLAN工作量大，可以使用VTP协议，把一台交换机配置成VTP Server, 其余交换机配置成VTP Client,这样他们可以自动学习到server 上的VLAN 信息。

> 摘抄自 [百度百科-vtp](https://baike.baidu.com/item/VTP)

### vtp模式

![](https://i.postimg.cc/m2C3Kxpc/2019-11-04-101337.png)

* https://blog.csdn.net/hexinblog/article/details/15498965  vtp实例

* https://baijiahao.baidu.com/s?id=1597438756537946544&wfr=spider&for=pc

* https://jingyan.baidu.com/article/5552ef47e6728e518ffbc9cc.html

`int range` 是进入一组端口，这组端口具要进行相同的配置，同时在这组端口上生效。
