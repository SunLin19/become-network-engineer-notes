# 简说UDP协议

UDP 是User Datagram Protocol的简称，中文名是用户数据报协议，UDP有不提供数据包分组、组装和不能对数据包进行排序的缺点，也就是说，当报文发送之后，是无法得知其是否安全完整到达的。当使用UDP协议传输信息流时，用户应用程序必须负责解决数据报丢失、重复、排序，差错确认等问题。所以，UDP是一种<ins>无连接的传输层协议</ins>，提供<ins>面向事务的简单不可靠信息传送服务</ins>。


理解网络编程方法以及流程图

* 格式
* 连接（伪代码）
* 编程

> 以下代码摘自 [xiao-apple36-Python UDP实例](cnblogs.com/xiao-apple36/p/9279108.html)，并作必要性的注释说明

server

```python
import socket
BUFSIZE = 1024 # 字符大小
ip_port = ('127.0.0.1', 9999)
server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # udp协议
server.bind(ip_port) #服务端绑定ip与端口
while True:
    data,client_addr = server.recvfrom(BUFSIZE) #接收UDP数据
    print('server收到的数据',data,'client地址',client_addr)
    server.sendto(data.upper(),client_addr)#发送UDP数据会给客户端（大写，地址端口（套接字））
 server.close()
```
client

```
import socket
BUFSIZE = 1024
client = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
while True:
    msg = input(">> ").strip()
    ip_port = ('127.0.0.1', 9999)
    client.sendto(msg.encode('utf-8'),ip_port) # 将消息发送给服务端
    data,server_addr = client.recvfrom(BUFSIZE)
    print('客户端recvfrom ',data,'server地址',server_addr)
 
client.close()
```


[Python3 网络编程](https://www.runoob.com/python3/python3-socket.html)

> 图片摘自[UDP协议图文详解-Linux社区](https://www.linuxidc.com/Linux/2018-09/154366.htm)

![](https://i.postimg.cc/rsyz9qX9/180924145972612.jpg)
