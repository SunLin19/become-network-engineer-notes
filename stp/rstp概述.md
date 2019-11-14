# rstp概述

## 传统STP的问题

我觉得Jan Ho的说明例子很明确：高延迟的网络用来提供证券交易服务 (所谓秒秒钟几十万上落)，或者用作紧急电话通讯，会容易导致因时效性过低，投入资金过多，集体资金流失的不满动荡。

## 加速的改进

### 从监听到学习的时间 

STP 预设 Listening 和 Learning 为 15 秒 (Forward Delay)，Port 由 Down 至 Up 一共等待 30 秒。而 RSTP 则使用 Synchronization 的概念，Switch 并非被动地等待 Root Switch 传来的 BPDU 讯息在判断 Topology，取而代之的是主动去跟相邻的 Switch 沟通，尽快把网络 Topology 传开去。这就能省却那 30 秒的被动学习时间，稍後详述。



写作思路

端口状态的时间转变过程

再述uplinkfast与backbonefast
