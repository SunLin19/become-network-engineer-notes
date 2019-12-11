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
R3(config)#access-list 1 deny host 192.168.1.1
R3(config)#access-list 1 permit any
```




