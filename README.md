# ip-book
继《这本书能让你连接互联网》又一新书，同时也是回归互联网之作，持续更新，等到较为完善之时，考虑docsify归纳成书，书名《？？？？》还没想好，

《从零开始理解网络》？可能吧。。。

### 计划

* [x] 双休日完善（先放出参考链接）7.13-7.14
* [x] 周一-周五（理解网关）7.19
* [x] 周四-周六（集线器与交换机）7.25-7.27
* [x] 周三-周六（OSI简略模型概览）8.1-8.3
* [ ] 周日-周五（http协议） 8.4-8.9

### 目录


* 前言 （暂时不写）

* 发展史 
  * [计算机网络发展过程概览](history/计算机网络发展过程概览.md) （已完成）
  
* OSI概览
  * [OSI简略概览](osi-intro/OSI模型简略概览.md) （已完成）
  
* TCP/IP模型各层协议略览
  * 数据链路层：[arp协议](link/简说arp协议.md) （计划中）

* IP地址、子网掩码、网关
  * [ip地址的设计](ip-sm-gw/IP地址的设计.md)（已完成）
  * [子网掩码的作用](ip-sm-gw/子网掩码的作用.md)（已完成）
  * [网关的意义](ip-sm-gw/网关的意义.md)（已完成）
* 。。。
* 后记（不被承认的无奈）


## 目录含义&随想短语

发展史：

* 了解历史可以知道他们出于什么目的而设计或开发某一功能模块的（为什么要这样设计）
* 理解OSI模型的作用及意义，对后面的网络学习非常重要


### 参考链接


前置名词

[帧、数据报、数据包的区别和联系](https://blog.csdn.net/qq_25606103/article/details/51295965)

[物理层的基本概念和数据通信基础](https://www.cnblogs.com/yangmingxianshen/p/7796386.html)


IP、子网掩码、网关

* [ip地址0.0.0.0是什么意思 - segmentfault](https://segmentfault.com/q/1010000003732310)

* [关于A类地址，B类地址，C类地址的划分和详细理解](https://blog.csdn.net/qq_40160605/article/details/84667066)

* [IP地址为什要分类？就是a类,b类,c类。。。?](https://www.zhihu.com/question/31766172)

* [IP地址，子网掩码，默认网关，DNS服务器详解](https://www.cnblogs.com/JuneWang/p/3917697.html)

* [如何计算网络地址和广播地址](https://blog.csdn.net/lzh657083979/article/details/77606217)

* [【科普】IP地址详解-你,是谁？(二）](https://zhuanlan.zhihu.com/p/26098552)

* [子网掩码的计算及与子网数、主机数关系](https://blog.csdn.net/yinshitaoyuan/article/details/51782330)

* [网络位和主机位怎么分的?](https://zhidao.baidu.com/question/26770723.html)

* [百度百科主机地址](https://baike.baidu.com/item/%E4%B8%BB%E6%9C%BA%E5%9C%B0%E5%9D%80)

* [IBM® IBM Knowledge Center - IPv4 与 IPv6 的比较 ](https://www.ibm.com/support/knowledgecenter/zh/ssw_ibm_i_71/rzai2/rzai2compipv4ipv6.htm#rzai2compipv4ipv6__compaddress) 

* [MBA智库 - 网关](https://wiki.mbalib.com/wiki/%E7%BD%91%E5%85%B3)

* [要想实现两台电脑通讯，如何修改网络ip地址及默认网关？](http://www.360doc.com/content/18/0409/13/47178282_744151500.shtml)

* [问cisco里的"特权模式"和"全局模式"有什么区别?](https://zhidao.baidu.com/question/92946651.html)


网络设备


* [多种场景的网桥应用案例](https://www.sohu.com/a/237267116_657991)

* [无线网桥应用场景](https://jingyan.baidu.com/article/1876c85255a529890b137681.html)

* [无线AP与路由器的区别](https://www.diangon.com/m430171.html)

* [无线AP图](http://www.sohu.com/a/256460042_374240)

* [中继器和集线器的区别](http://www.hqps.com/tech/201411/219528.html)

* [集线器的作用是什么 和交换机有什么区别](http://m.365azw.com/share/117006)

* [交换机的广播风暴](https://blog.csdn.net/u010486124/article/details/31803263)

* [交换机的工作原理？](https://www.zhihu.com/question/34736235)

* [交换机工作原理、MAC地址表、路由器工作原理详解](https://www.cnblogs.com/gopark/p/8980783.html)

* [物理层的基本概念和数据通信基础](https://www.cnblogs.com/yangmingxianshen/p/7796386.html)

* [电梯无线网桥监控方案](https://zhuanlan.zhihu.com/p/38515313)

* [二层交换机与三层交换机区别详解！](https://www.cnblogs.com/felixzh/p/8690036.html)

* [二层、三层、四层交换机、路由器的区别](https://network.pconline.com.cn/474/4741916.html)

* [OSI参考模型——网络层：路由选择算法和协议](https://blog.csdn.net/jeffleo/article/details/53965708)


网络连接

http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html

https://www.zhihu.com/question/24002080

https://wizardforcel.gitbooks.io/network-basic/0.html


传输层

[计算机网络学习笔记--传输层知识总结](https://www.cnblogs.com/fingerboy/p/5402354.html)

OSI

* [OSI七层模型与TCP/IP五层模型](https://www.cnblogs.com/qishui/p/5428938.html)

* [OSI七层协议模型和TCP/IP四层模型](https://blog.csdn.net/freeking101/article/details/77977941)

* [OSI/RM七层模型 和 TCP/IP四层模型](https://my.oschina.net/tita/blog/3053424)

* [OSI协议和TCP/IP协议模型](https://blog.csdn.net/zhydream77/article/details/81700535)

* [牛客-物理层的作用（概览）](https://www.nowcoder.com/questionTerminal/527819d3161d40229ff0d1e3b78f8d57)

* [应用层、表示层、会话层、传输层、网络层、数据链路层、物理层](https://blog.csdn.net/weixin_41738417/article/details/92796077)


传输层

* [理解面向连接和无连接协议之间的区别](https://michaelyou.github.io/2015/03/24/%E7%90%86%E8%A7%A3%E9%9D%A2%E5%90%91%E8%BF%9E%E6%8E%A5%E5%92%8C%E6%97%A0%E8%BF%9E%E6%8E%A5%E5%8D%8F%E8%AE%AE%E4%B9%8B%E9%97%B4%E7%9A%84%E5%8C%BA%E5%88%AB/)
