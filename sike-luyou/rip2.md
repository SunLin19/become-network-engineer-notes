# rip2协议

## rip与rip2

rip与rip2的区别表，这也是有类和无类路由的区别。

> 摘自 [rip1与rip2的区别](https://zhidao.baidu.com/question/5214089.html)，并参考了[有类路由和无类路由有什么区别？](https://learningnetwork.cisco.com/thread/15719)

![](https://i.postimg.cc/SxRNHSJk/b660.png)

rip2在动态学习的跨路由访问基础上，又增强及优化了传输策略，从这也可以看出推出rip2实际上是给路由信息协议续命。

## 实验拓扑配置



拓扑需要改进 按书上来


路由间分享信息，端口连接传送，数据保存在路由器，在借由路由器转发其他端口，network解释

![](https://i.postimg.cc/tRnFZ2qM/Snipaste-2019-10-15-21-38-09.png)

R1

```ios
R1(config)#int loopback 0
R1(config-if)#ip add 172.16.1.3 255.255.0.0
R1(config-if)#int s0/1/0
R1(config-if)#ip add 10.1.1.1 255.255.255.0
R1(config-if)#no shutdown
```
R2

```ios
R2(config)#int loopback 0
R2(config-if)#ip add 192.168.1.71 255.255.255.192
R2(config-if)#int s0/1/1
R2(config-if)#ip add 10.1.1.2 255.255.255.0
R2(config-if)#no shutdown
```

rip version2

```ios
R1(config-if)#router rip
R1(config-router)#version 2
R1(config-router)#no auto-summary 
R1(config-router)#network 172.16.0.0
R1(config-router)#network 10.0.0.0

R2(config)#router rip
R2(config-router)#version 2
R1(config-router)#no auto-summary 
R2(config-router)#network 10.0.0.0
R2(config-router)#network 192.168.1.0 //对比以下ABC类型IP地址的网络位与主机位的参照表
```

|IP地址类型|地址表示范围|缺省（默认）子网掩码|网络位n与主机位h
|:-:|:-:|:-:|:-:|
|A类地址：|1.0.0.1-127.255.255.255|255.0.0.0|0nnnnnnn.hhhhhhhh.hhhhhhhh.hhhhhhhh
|B类地址：|128.0.0.1-191.255.255.255|255.255.0.0|10nnnnnn.nnnnnnnn.hhhhhhhh.hhhhhhhh
|C类地址：|192.0.0.1-223.255.255.255|255.255.255.0|110nnnnn.nnnnnnnn.nnnnnnnn.hhhhhhhh

### 记录以及解答

no auto-summary：

RIPv2版本中，打个比方吧，R1现在有一条10.1.1.2/24的路由，如果在RIPv2版本中开启了`no auto-summary`，那么RIPv2在传输这条路由的时候就会为10.1.1.0/24传出去；如果没有关闭`auto-summary`，那么就会以10.0.0.0/8的路由形式传出去。

> 摘自 [no auto summary的作用－－学习笔记](https://blog.51cto.com/zcry21cn/1740894)

network ip地址：

执行指令后，路由器将network通告的网段交给下一直连路由分享（端口连接传送），下一直连路由并保存数据在路由器内，如此层层转发分享，实现和远端非直连网络建立通信。**动态路由协议就是路由器之间分享路径信息的协议。**

> 参考 《趣学CCNA——路由与交换》 动态协议概述部分

`% 10.0.0.0 overlaps with Serial0/1/1`：

在同一路由器上，不能存在涵盖已分配的CIDR更大范围的CIDR，例如：在同一路由的不同串口分别配置10.1.1.2/24、10.2.2.2 255.0.0.0





### 切入点

或者说能实现自动汇总的前提掩码是要一样，保证网关的覆盖范围。

利用前面的子网掩码章节做实验

https://zhidao.baidu.com/question/540720999.html

https://zhidao.baidu.com/question/35015394.html

https://blog.51cto.com/zcry21cn/1740894

https://www.zhihu.com/question/264432033

* [思科模拟器rip2协议的配置_](https://jingyan.baidu.com/article/17bd8e522e5b8c85ab2bb8d2.html)
* [路由协议的自动汇总](https://zhidao.baidu.com/question/521435066.html)


* int s0/0/1 与 int s2/0 根据设备的不同表达形式不太一样。
* interface LoopBack0 这是全称。 应该叫做回环口。相当于在设备内部创建了一个pc，且永远在线，也就是状态永远up。用于测试、一些特殊应用里

