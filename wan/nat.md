# NAT(Network Address Translation)

简单的来说，NAT是将私有IP地址通过边界路由转换成外网IP地址，在边界路由的NAT地址转换表中记录下这个转换，当外部数据返回时，路由使用NAT技术查询NAT转换表，再将目标地址替换成内网用户IP地址。

> 摘自 [[CCNA图文笔记]-31-NAT协议实例详解](https://www.qingsword.com/qing/745.html)

总结这段话，inside 先做 routing 再 nat，outside 先 nat，再做 routing。 

## NAT配置

![](https://i.postimg.cc/j2mt5b2P/6-UMbqjj-JGv.png)


### 静态NAT配置

**静态NAT就是一对一的NAT，内部有多少私有地址需要连接外网，就配置等量的外网IP与其对应。** 通过在R1上配置静态NAT，让VPC1和VPC2通过两个静态分配的公网IP和R2通信。

首先将交换机上的PC配置IP地址，并将交换机设为访问模式，使PC-1和PC-2通信。

```
//VPCS选GNS3 VM，set pcname xxxx 即为设置PC名称
//配置PC的IP地址与网关
PC-1> ip 192.168.1.1 24 192.168.1.254
PC-2> ip 192.168.1.2 24 192.168.1.254
```

```
SW1(config)#int range e0/0-2 
SW1(config-if-range)#swi //三层交换机，需要开启二层数据链路端口
SW1(config-if-range)#swi mode access
SW1(config-if-range)#no sh
```

在边界路由做NAT

```
R1(config)#int f0/0
R1(config-if)#ip add 192.168.1.254 255.255.255.0 //配置内网网关
R1(config-if)#no sh
R1(config-if)#ip nat inside //配置内网入口

//配置与ISP相连的外网接口
R1(config-if)#int s1/0
R1(config-if)#ip add 11.11.11.11 255.255.255.0
R1(config-if)#no sh
R1(config-if)#ip nat outside //配置外网入口

/*配置静态转换条目，每个内网IP需要和一个外网IP对应*/
R1(config)#ip nat inside source static 192.168.1.1 11.11.11.10
R1(config)#ip nat inside source static 192.168.1.2 11.11.11.20
```


参考链接：

[百度文库-网络地址转换(NAT)](https://wenku.baidu.com/view/032e0a65f5335a8102d22001.html)

[在思科模拟路由器上怎么配置静态NAT](https://zhinan.sogou.com/guide/d316513559506.htm?ch=zn.xqy.related.pc)

[[CCNA图文笔记]-31-NAT协议实例详解](https://www.qingsword.com/qing/745.html)
