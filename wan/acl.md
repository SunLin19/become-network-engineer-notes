# 访问控制列表

## acl概述

ACL的作用及意义：控制数据流量经过路径的准入准出，以达到网络安全及稳定可靠、数据资料具有较高保密的目的。ACL类型也有很多如IP、二层和基于其他协议的ACL。应用最为广泛的为标准与扩展ACL。

* 标准ACL：检查源IP地址、允许或拒绝整个IP协议
* 扩展ACL：检查源IP地址和目的IP地址、允许或拒绝某个指定协议

ACL语序与层层的`if-else`结构顺序一致，出站流程同理。

> 图摘自[华三(H3C)原厂培训_第07章_ACL原理和基本配置](https://wenku.baidu.com/view/cc6cfa5c02d276a201292e39.html)

![](https://i.postimg.cc/zGhKPvQr/10-21-55.png)

## acl语法规则

标准版：`access-list {1-99} {permit | deny} source-addr [source-wildcard]`

* acl使用编号:
  * 标准版：（1 ~ 99）与（1300 ~ 1999）
  * 扩展版：（100 ~ 199）和（2000 ~ 2699）
* 关键字：***permit*** 允许流量进出；***deny*** 拒绝流量进出
* 通配符即反掩码，掩码位取反

扩展版：`access-list {100-199} {permit | deny} protocol source-addr [source-wildcard] [operator operand] destination-addr [destination-wildcard] [operator operand] [established]`

## acl实例构建

![](https://i.postimg.cc/Qdjr4fmJ/5-01-47.png)


<details><summary> click me! </summary>

```
R1(config)#int s1/0 
R1(config-if)#ip add 192.168.1.2 255.255.255.0
R1(config-if)#no sh
                      
R1(config-if)#int s1/1
R1(config-if)#ip add 192.168.10.3 255.255.255.0
R1(config-if)#no sh
```

```
R2(config)#int s1/0
R2(config-if)#ip add 192.168.1.1 255.255.255.0
R2(config-if)#no sh
//0.0.0.0 代表的是所有任意的意思
//所有网络到达路径经过192.168.1.2转发
R2(config-if)#ip route 0.0.0.0 0.0.0.0 192.168.1.2 
```

```
R3(config)#int s1/0
R3(config-if)#ip add 192.168.10.4 255.255.255.0
R3(config-if)#no sh
R3(config-if)#ip route 0.0.0.0 0.0.0.0 192.168.10.3
R3(config)#do ping 192.168.1.1
```
</details>



### acl测试

创建编号为1的ACL标准版，阻止192.168.1.1连接到192.168.10.4

```
/**
两个指令顺序不能颠倒，如果将允许任何IP地址放在第一条，
那么由于每个IP地址都能匹配，第二条将永远不会被执行到
*/
R3(config)#access-list 1 deny host 192.168.1.1
//所有acl在尾处都隐藏拒绝所有项，若没有permit any将拒绝所有流量
R3(config)#access-list 1 permit any

/**
 * 将编号是1的标准ACL应用到路由器的s0/1接口上
 * 方向是in，即进入的方向；反之，out则为出去的方向
 */
R3(config)#int s1/0
R3(config-if)#ip access-group 1 in
R3(config-if)#end
```

此时双方的IP地址已经ping超时了，接着查看acl条目

```
R3(config)#do sh access-li
Standard IP access list 1 //ACL类型和编号
    10 deny   192.168.1.1 (30 matches) //1行为1条目,其中"10"是行号
    20 permit any (30 matches) //matches表示匹配的次数
```

可以使用下面的命令来删除标准ACL中对应的条目，如不需复用，可使用`no access-list 1`直接取消配置效果并删除ACL1。

```
/*首先进入标准ACL 1的编辑模式*/
R3(config)#ip access-list standard 1
R3(config-std-nacl)#no 10  /*删除行号是10的这一行*/
R3(config-std-nacl)#do sh access-li
Standard IP access list 1
    20 permit any (30 matches)

//取消接口上调用的acl
R3(config)#int s1/0
R3(config-if)#no ip access-group 1 in
```

### 扩展acl

在R3上配置远程登陆的虚拟端口（VTY）

```
//路由器上从0-4有5个VTY，同时配置这5个端口 `
R3(config)#line vty 0 4
R3(config-line)#password 123
R3(config-line)#login
R3(config-line)#end
```

ACL配置拒绝R2到R3的Telnet通信

```
/*
 * 扩展ACL号为100，第一条命令拒绝tcp类型的连接，
 * 源主机是192.168.1.1，没有配置源端口，默认就匹配所有端口，
 * 目的主机是192.168.10.4，目的端口是23，即telnet
 */
R1(config)#access-list 100 deny tcp host 192.168.1.1 host 192.168.10.4 eq telnet
//允许所有IP通信，源和目的是任意主机
R1(config)#access-list 100 permit ip any any

//启用ACL
R1(config)#int s 1/0
R1(config-if)#ip access-group 100 in
R1(config-if)#end

```

恢复R2到R3的telnet通信

```
	/*
 * 末尾添加established参数，
 * 注意，因为是允许R3访问R1的telnet，
 * 源和目的地址以及端口别搞错了，
 * 这个访问列表是当R3的telnet发到R1，R1回复的TCP中携带ACK号才匹配的，
 * 所以源应该是R1，源端口应该是R1的telnet端口23，
 * 目的端口是R3上的一个随机端口，没有写出来就是匹配所有端口。
 */

```










