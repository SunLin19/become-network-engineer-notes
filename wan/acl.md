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
  * 标准版：1~99与1300~1999
  * 扩展版：100~199和2000~2699
* 关键字：***permit*** 允许流量进出；***deny*** 拒绝流量进出
* 通配符即反掩码，掩码位取反

扩展版：`access-list {100-199} {permit | deny} protocol source-addr [source-wildcard] [operator operand] destination-addr [destination-wildcard] [operator operand] [established]`
