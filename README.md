# 成为网络工程师笔记

### 笔记要达到的目标

即使是零基础的，也能通过阅读该笔记，找到从事网络行业的相应工作。这同时也是自我的网络学习笔记，从桌面运维转向网络管理的行动。


记录：在理解配置以及深入原理的过程中，发现CCNA还是满足于配置的熟练，但对于更深层次的规划与设计网络（如：路由重发）的细节，要更改原有网络只知道配置还是不行的，没有这类人才公司将花费巨大的成本寻找高资质的外包（招标一年也多是百万级别），NP的价值就已经体现出来了（对比每年外包花个百来万，养员工月来万实在划算得多...）。

最近开始备考，觉得：在写作的同时，还要看红茶的视频放松，以及周末的刷题


### 待写作的问题记录

网络规划问题

https://mbd.baidu.com/newspage/data/landingsuper?context=%7B%22nid%22%3A%22news_9811919299638948526%22%7D&n_type=1&p_from=4


### 目录

#### 前言

* 非正式的前言
  * [对以前处于学生时期的学习反思](my-study/对以前处于学生时期的学习反思.md)
  * [求职期间的回顾与分析](my-study/求职期间的回顾与分析.md)

#### 小宇宙

* 发展史、OSI概览
  * [计算机网络发展过程概览](history/计算机网络发展过程概览.md)
  * [OSI简略概览](osi-intro/OSI模型简略概览.md)
  
* TCP/IP各层部分相关协议及功能略览  
  * 应用层：[简述DHCP](L/L5/简述dhcp.md) 
  * 传输层：[简说UDP协议](L/L4/简说UDP协议.md) 
  * 网络层：[arp协议](L/L3/简说arp协议.md) 
  * 数据链路层：
    * [共享介质、交换式以太网、VLAN](L/L2/共享介质、交换式以太网、VLAN.md) 
    * [以太网格式](L/L2/以太网格式.md) 
  * 物理层：[网线综合知识](L/L1/网线综合知识.md)
  * 各层功能回顾：[TCP/IP各层功能回顾](L/TCP-IP各层功能回顾.md)
  
#### 思科实验  

* IP地址、子网掩码、网关
  * [ip地址的设计](ip-sm-gw/IP地址的设计.md)
  * [子网掩码的作用](ip-sm-gw/子网掩码的作用.md)
  * [网关的意义](ip-sm-gw/网关的意义.md)
  * [CIDR与VLSM](ip-sm-gw/CIDR与VLSM.md) 
  * [组播](supplement/组播.md) 
  
* 网络路由
  * [静态路由](sike-luyou/静态路由.md)
  * [rip协议配置](sike-luyou/rip协议配置.md)
  * [rip2协议](sike-luyou/rip2.md)
  * [EIGRP](sike-luyou/eigrp.md)
  * [EIGRP简要配置](sike-luyou/eigrp简要配置.md)
  * [ospf](sike-luyou/ospf.md)
  * [ospf配置](sike-luyou/ospf配置.md)

* 网络交换
  * [核心层、汇聚层、接入层](supplement/核心层、汇聚层、接入层.md)
  * [vlan、trunk、vlan间路由](sike-jiaohuan/vlan、trunk、vlan间路由.md)
  * [虚拟局域网干道协议](sike-jiaohuan/vtp.md)
  * [交换机过程、冗余、生成树](sike-jiaohuan/交换机过程、冗余、生成树.md)


* 生成树专题
  * [生成树协议概览](stp/生成树协议概览.md)
  * [pvst](stp/pvst.md) 
  * [rstp](stp/rstp.md) 
  * [mstp](stp/mstp.md)

* 广域网
  * 帧中继 计划中


## 相关资源分享

云盘资源

* [CISCO-Aspire Game](https://mega.nz/#!LTojASRT!rk5A3-m2Ep9ht6X_ukt1tWNkxgkQVeyEbmKgdKezH9g)
* [计算机网络原理flash动画](https://mega.nz/#!zaIVSQSJ!W6qoXLmTGy3LCJeoLMI1uEJGK6Yc0b-nvQy6p2ZBEQ4)

文档文献

 * [IBM® IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/zh/)
 * [Python 3 教程](https://www.runoob.com/python3/python3-tutorial.html)
 * [图解TCP/IP（第5版）](https://ccie.lol/knowledge-base/pdf-diagram-tcp-ip/)
 * [《IP路由协议疑难解析》](https://cread.jd.com/read/startRead.action?bookId=30383211&readType=1)
 * [Jan Ho 的网络世界](https://www.jannet.hk/zh-Hans/)
 * [思科技术支持](https://www.cisco.com/c/zh_cn/tech/index.html)
 * [H3C交换机 典型配置举例-6W100](http://www.h3c.com/cn/d_201312/807748_30005_0.htm)
 * [computernetworkingnotes](https://www.computernetworkingnotes.com/ccna-study-guide/ospf-neighbor-states-explained-with-example.html)
 * [红茶三杯ccietea](http://ccietea.com/)
 * [omnisecu知识库](http://www.omnisecu.com/cisco-certified-network-associate-ccna/what-is-ospf-metric-value-cost-and-ospf-default-cost-reference-bandwidth.php)
 * [Java，IT运维，服务器架设](http://www.davis-wiki.com/doku.php?id=Start)
 * [CCNAイージス CCNA Web教科書更新](https://www.infraexpert.com/)
 * [网络设备配置技术](http://183.167.250.28:9020/Website/index.html)

<!--
继《这本书能让你连接互联网》又一新书，同时也是回归互联网之作，持续更新，等到较为完善之时，考虑docsify归纳成书，书名《？？？？》还没想好，《从零开始理解网络》？可能吧。。。-->
