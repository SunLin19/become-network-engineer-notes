# eigrp配置

autonomous system 自治系统，每个AS之间都相是一个独立的网络。EIGRP建立邻居必须和邻居又相同的AS号，否则邻居建立不成功，无法通信。不同的AS区域间，通过进行[路由重发布](https://baike.baidu.com/item/%E8%B7%AF%E7%94%B1%E9%87%8D%E5%88%86%E5%8F%91)进行路由的互访。

> 参考 [百度知道-路由协议EIGRP里面，AS号意义是什么。](https://zhidao.baidu.com/question/545021403.html?qbl=relate_question_0)

![](https://i.postimg.cc/MZRY8dk3/2019-10-20-160605.png)

首先先把所有接口信息以及EIGRP全部启用，并且查看信息；注：其他路由器配置，同理。

```IOS
Router>
Router>en
Router#conf t
Router(config)#hostname R1
R1(config)#interface f0/0
R1(config-if)#ip address 192.168.12.1 255.255.255.0
R1(config-if)#no sh
R1(config-if)#ip address 192.168.13.1 255.255.255.0
R1(config-if)#no sh
R1(config-if)#exit
R1(config)#router eigrp 1 //1即是AS号
R1(config-router)#no auto-summary
R1(config-router)#network 192.168.12.0 0.0.0.255 //反掩码，即子网掩码二进制取反；如：1为0，0为1
R1(config-router)#network 192.168.13.0 0.0.0.255
R1(config-if)#end
R1#
```

度量值公式：`256*{K1（10^7/带宽）+K2（10^7/带宽）/（256-负载）+K3（延迟）/10+K5/(可靠性+K4)}`
* **默认情况下，K1和K3是1，其他的K值都是0，通常情况下：度量值=256×（10^7/最小带宽+累积延时/10）**
* 可行距离(FD)=被通告距离(AD)+该路由器到这个邻居路由器的度量值(metric)

`show ip eigrp topology` 因其FD距离一致，EIGRP自动实现负载均衡

![](https://i.postimg.cc/JngHFy5B/2019-10-20-170008.png)

https://blog.51cto.com/r1cky/1774644

https://www.jannet.hk/zh-Hans/post/enhanced-interior-gateway-routing-protocol-eigrp/
