# 帧中继

## 帧中继概述

电信运营商一般提供三种接入服务：互联网访问、二层专线访问、三层VPN访问

普通用户通常访问互联网，运营商给用户提供上网的设备，用户动态拨号上网，这个没太多的说法。

企业网络（专线）与家庭宽带体验差别：专线顾名思义，可以理解为一条企业专属线路，而家庭宽带则是公共线路；专线的上下行速度一致的，而家庭宽带上行波动偏差很大。另外，专线的价格和家庭宽带价格相差很大，同样100M的，家庭宽带可能130左右一个月，但专线的话估计要上3000左右，两者的价格相差了20倍不止。

Frame Relay（帧中继）是一个比专线经济的配置办法。假设一家公司有两个办公区，最简单的方法就是用Dedicated Line (专线) 把两点连接起来。如果这家公司未来增加办公区，就要再增加两条专线将其接通。

![](https://i.postimg.cc/C1cv8B0b/4-27.png)

但一条专线价值不菲，此外还必然占用路由器为数不多的接口，也增加了使用成本，显然并非理想的解决办法。帧中继的出现就是让我们可以通过ISP提供的该项服务，与不同位置点建立连线。ISP用Frame Relay Network同时服务多个客户，价格当然比专线便宜，且每个路由器仅需一个接口，两者都可以在预算上省成本。

![](https://i.postimg.cc/YCMBQPm7/14-01.png)

帧中继发展的必要条件
* 光纤传输线路的使用
随着光纤传输线路的大量使用，数据传输质量大大提高，光纤传输线路的误码率一般低于 。也就是说在通信链路上很少出现错码，即使偶尔出现的误码也可以由终端处理和纠正。
* 用户终端的智能化
用户终端的智能化（比如计算机的使用），使终端的处理能力大大增强，从而可以把分组交换网中由交换机完成的一些功能（如流量控制、纠错等）能够交给终端去完成。

正由于具备这两个必要条件，使得帧中继交换机可省去纠错控制等功能，从而使其操作简单，即降低了费用又减少了时延，提高了信息传输效率又能保证传输质量。




## 帧中继实例

![](https://i.postimg.cc/h4J56PD1/14-03.png)

R2同理。

```
//进入（serial）串口设定IP地址
R1(config)#int s1/1
R1(config-if)#ip add 192.168.10.2 255.255.255.0
//封装帧中继格式来让接口知道使用frame-relay协定
R1(config-if)#enc frame-relay 
//serial restart-delay 连续重启延时，0代表不重启，比如30代表30秒后重启路由器
R1(config-if)#serial restart-delay 0
//在GNS3中配置Frame relay switch发送方端口与DLCL
R1(config-if)#frame-relay interface-dlci 101
//思科默认是关闭接口的，给它开启
R1(config-if)#no sh
```

参考文献

* [知乎-大公司是怎么接入网络服务的？](https://www.zhihu.com/question/318806738)
* [googlebook-电子商务概论](https://books.google.nl/books?hl=zh-CN&id=OuF0DwAAQBAJ&q=frame+relay)
* [张曾科-计算机网络](https://books.google.nl/books?id=gUmThRY3RHEC&pg=PA178&lpg=PA178&dq=%E5%B8%A7%E4%B8%AD%E7%BB%A7%E6%A0%BC%E5%BC%8F+%E5%9B%BE%E8%A7%A3)



DTE与DCE的概念与区别：

* DTE(Data Terminal Equipment)数字终端设备，指一般的终端或是计算机、打印机等用户端设备
* DCE(DataCircuit-terminatingEquipment)，数字通信设备，通常指调制解调器，多路复用器或数字设备
* DCE一方提供时钟，DTE不提供时钟，但它依靠DCE提供的时钟工作，比如PC与MODEM的连接



多台帧中继组成的云需要变更为[NNI](https://baike.baidu.com/item/NNI/5234091)接口类型传输。