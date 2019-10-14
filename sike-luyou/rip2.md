# rip2协议

## rip与rip2

rip与rip2的区别表，这也是有类和无类路由的区别。

> 摘自 [rip1与rip2的区别](https://zhidao.baidu.com/question/5214089.html)，并参考了[有类路由和无类路由有什么区别？](https://learningnetwork.cisco.com/thread/15719)

![](https://i.postimg.cc/SxRNHSJk/b660.png)

rip2在动态学习的跨路由访问基础上，又增强及优化了传输策略，从这也可以看出推出rip2实际上是给路由信息协议续命。


### 切入点

* [思科模拟器rip2协议的配置_](https://jingyan.baidu.com/article/17bd8e522e5b8c85ab2bb8d2.html)
* [路由协议的自动汇总](https://zhidao.baidu.com/question/521435066.html)


* int s0/0/1 与 int s2/0 根据设备的不同表达形式不太一样。
* interface LoopBack0 这是全称。 应该叫做回环口。相当于在设备内部创建了一个pc，且永远在线，也就是状态永远up。用于测试、一些特殊应用里
