# rip2协议

## rip与rip2

rip与rip2的区别表，这也是有类和无类路由的区别。

> 摘自 [rip1与rip2的区别](https://zhidao.baidu.com/question/5214089.html)，并参考了[有类路由和无类路由有什么区别？](https://learningnetwork.cisco.com/thread/15719)

![](https://i.postimg.cc/SxRNHSJk/b660.png)

rip2在动态学习的跨路由访问基础上，又增强及优化了传输策略，从这也可以看出推出rip2实际上是给路由信息协议续命。

## 实验拓扑配置



拓扑需要改进 按书上来


路由间分享信息，端口连接传送，数据保存在路由器，在借由路由器转发其他端口，network解释

![](https://i.postimg.cc/NMqBSbW5/2019-10-16-013945.png)

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

```

R3

```ios
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

关于 `% 10.0.0.0 overlaps with Serial0/1/1` 报错：

在同一路由器上，不能存在涵盖已分配的CIDR更大范围的CIDR，例如：在同一路由的不同串口分别配置10.1.1.2/24、10.2.2.2/8

![](https://i.postimg.cc/j5dH4HR9/16s.png)

> 参考 [in routes why i cannot set two ip address in same network on two serial interfaces ?](https://learningnetwork.cisco.com/thread/64294)
