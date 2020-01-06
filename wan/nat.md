# NAT

简单的来说，NAT是将私有IP地址通过边界路由转换成外网IP地址，在边界路由的NAT地址转换表中记录下这个转换，当外部数据返回时，路由使用NAT技术查询NAT转换表，再将目标地址替换成内网用户IP地址。

> 摘自 [[CCNA图文笔记]-31-NAT协议实例详解](https://www.qingsword.com/qing/745.html)

总结这段话，inside 先做 routing 再 nat，outside 先 nat，再做 routing。 

参考链接：

[百度文库-网络地址转换(NAT)](https://wenku.baidu.com/view/032e0a65f5335a8102d22001.html)

[在思科模拟路由器上怎么配置静态NAT](https://zhinan.sogou.com/guide/d316513559506.htm?ch=zn.xqy.related.pc)

[[CCNA图文笔记]-31-NAT协议实例详解](https://www.qingsword.com/qing/745.html)
