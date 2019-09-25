# 略览以太网

网络拓扑结构的参考文章

* [百度经验-认识计算机网络拓扑结构](https://jingyan.baidu.com/article/09ea3ede69b7c4c0aede39d1.html)
* [百度百科-计算机网络拓扑结构](https://baike.baidu.com/item/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%E6%8B%93%E6%89%91%E7%BB%93%E6%9E%84/10230127)

## 网卡

每台通过网线通讯的计算机都需要安装一个硬件设备—— 网卡 ( NIC )， NIC 是 Network Interface Controller 的缩写。

从物理的层面看，网卡负责将比特流转换成电信号发送出去； 反过来，也负责将检测到的电信号转换成比特流并接收。

从软件的层面看，发送数据时，内核协议栈负责封装以太网帧(填充 目的地址 ， 源地址 ， 类型 和 数据 并计算 校验和)，并调用网卡驱动发送； 接收数据时，负责验证 目的地址 、 校验和 并取出数据部分，交由上层协议栈处理。

> 摘自 [陈彦霏 linux-network-programming](https://linux-network-programming.readthedocs.io/zh_CN/latest/protocols/ethernet.html)

个人理解：从硬件功能描述来看网卡在执行物理层功能，从软件运行目的来说网卡是在执行数据链路层负责的功能。但传送和识别数据帧这是物理层不具有的，所以网卡还是属于数据链路层。

### 共享介质型以太网

**关键词：CSMA、CSMA/CD**

共享介质网络指由多个设备共享同一通信介质的一种网络（总线型网络）。共享介质型网络中有两种介质访问控制方式：

* 争用方式
* 令牌传递

#### CSMA 载波监听多路访问

载波监听多路访问采用总线型拓扑设计，即是先抢先得的方式占用信道发送数据；若是多个主机同时发送帧，则会产生冲突的现象，造成网络拥堵。


### 交换式以太网

[百度百科-交换式以太网](https://baike.baidu.com/item/%E4%BA%A4%E6%8D%A2%E5%BC%8F%E4%BB%A5%E5%A4%AA%E7%BD%91/188374?fr=aladdin)

交换式以太网不需要改变网络其它硬件，包括电缆和用户的网卡，仅需要用**交换式交换机改变共享式HUB**，节省用户网络升级的费用。

参考连接

* [以太网（数据链路层）学习笔记](https://blog.csdn.net/u014492609/article/details/51285935)
* [陈彦霏 linux-network-programming](https://linux-network-programming.readthedocs.io/zh_CN/latest/protocols/ethernet.html)
* [晴刃- 晴刃 ccna 文章归档](https://www.qingsword.com/sitemap.html#ccna)
