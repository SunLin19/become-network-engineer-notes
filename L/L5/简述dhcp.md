# 简述DHCP

如果逐一为每台主机配置IP地址会非常繁琐，为了实现自动化统一管理ip地址分配，就产生了动态主机设置协议（英语：Dynamic Host Configuration Protocol，缩写：DHCP）；它是一个用于局域网的网络协议，位于OSI模型的应用层，使用UDP协议工作，主要有两个用途：

* 用于内部网或网络服务供应商自动分配IP地址给用户
* 用于内部网管理员作为对所有计算机作中央管理的手段

## DHCP原理

> 摘自 [dhcp原理及其实现流程 - 电子发烧友](http://www.elecfans.com/baike/wangluo/luyouqi/20180306643883.html)

![](https://i.postimg.cc/44zhF27G/p-IYBAFqe-Z5q-AXTe-OAABe-E14sd-KY128.jpg)

DHCP请求IP地址的过程如下：
1. 主机发送DHCPDISCOVER广播包在网络上寻找DHCP服务器
2. DHCP服务器向主机发送DHCPOFFER单播数据包，包含IP地址、MAC地址、域名信息以及地址租期
3. 主机发送DHCPREQUEST广播包，正式向服务器请求分配已提供的IP地址
4. DHCP服务器向主机发送DHCPACK单播包，确认主机的请求

## DHCP简略配置

使用win server进行dhcp的安装配置 （关于IP地址，会在思科实验的IP、掩码、网关章节进行详细阐述）

![](https://i.postimg.cc/wTytv8BQ/Snipaste-2019-09-01-15-00-52.png)

将client端IP设为自动获取

![](https://i.postimg.cc/6q0N6Qvp/Snipaste-2019-09-01-16-44-29.png)

参考文章：

* [oracle- 系统管理指南：DHCP](https://docs.oracle.com/cd/E24847_01/html/819-7058/dhcp-overview-14a.html)
* [HUAWEI - DHCP](https://support.huawei.com/enterprise/zh/doc/EDOC1100058966/2ef50604)
* [wiki - 动态主机设置协议](https://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E4%B8%BB%E6%9C%BA%E8%AE%BE%E7%BD%AE%E5%8D%8F%E8%AE%AE)
* [DHCP](https://cshihong.github.io/2017/12/09/DHCP/)
