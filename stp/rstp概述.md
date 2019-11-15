# rstp概述

## 传统STP的问题

我觉得Jan Ho的说明例子很明确：高延迟的网络用来提供证券交易服务 (所谓秒秒钟几十万上落)，或者用作紧急电话通讯，会容易导致因时效性过低，投入资金过多，集体资金流失的不满动荡。

## 加速的改进

RSTP根据端口在活动拓扑中的作用，定义了3种端口角色（STP有5种角色）：禁用端口（Disabled Port）、根端口（Root Port）、指定端口（Designated Port）、**为支持RSTP的快速特性规定的替代端口（Alternate Port）和备份端口（Backup Port）**。

RSTP有五种端口类型。根端口和指定端口这两个角色在RSTP中被保留，阻断端口分成备份和替换端口角色。生成树算法（STA）使用BPDU来决定端口的角色，端口。

RSTP的主要功能可以归纳如下：

1. 发现并生成局域网的一个最佳树型拓扑结构
2. 发现拓扑故障并随之进行恢复，自动更新网络拓扑结构，启用备份链路，同时保持最佳树型结构

### 

https://baike.baidu.com/item/RSTP/2235256?fr=aladdin

### 从监听到学习的时间 

STP预设Listening和Learning为15秒 (Forward Delay)，端口由Down至Up，一共等待30秒。而RSTP则使用同步的概念，交换机并非被动地等待bridge priority传来的BPDU讯息再判断拓扑，取而代之的是主动去跟相邻的交换机沟通，尽快把网络拓扑传开去。这就能省却那30秒的被动学习时间，稍後详述。



写作思路

端口状态的时间转变过程

再述uplinkfast与backbonefast
