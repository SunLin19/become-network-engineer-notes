# pvst生成树实例

## 选举说明

PVST+ 是Cisco交换机预设的STP，通过PVST+，交换机可以建立一个无环路的拓扑。各交换会按程序完成以下步骤：

1. 在整个网络里选举一个Root Switch（Switch Priority或称Bridge Priority）
2. 除了 Root Switch 外的其他各 Switch 都选择一个 Root Port
3. 所有网段选择一个 Designated Port
4. 把没成为Root Port或Designated Port的端口成为Non-Desiganted Port，这些端口会被禁用，以去防止环路发生

交换机间各端口的选举规则:

若规则相比较数值对等，则由顶向下层层对比进行选举

* 规则1：比 Bridge ID，小的胜出 (只有选 Root Switch 时使用)
* 规则2：比 Root Cost，小的胜出
* 规则3：比对方的 Bridge ID，小的胜出
* 规则4：比对方的 Port ID，小的胜出

## 拓扑展示

我们先将各个交换机端口置为trunk模式进行互通，以便直观展示生成树实例，拓扑如下：

![](https://i.postimg.cc/ZKC1Nyj2/Snipaste-2019-11-08-20-57-25.png)

### 选举BP（Bridge Priority）

首先在拓扑中选举Root Switch，执行规则1，比Bridge ID，小的胜出，Bridge ID 即优先级和交换机mac地址共同构成的一组数值。

Switch Priority = 32768(默认优先值)+vlan id
![](https://i.postimg.cc/Qd00hcBV/Snipaste-2019-11-08-21-19-12.png)

图中bridge id处的address即为交换机的MAC地址，默认优先级等同情况下再比较mac地址选出Root Switch。


<!--
 
接下来，我们再创建一个vlan，并更改他们的默认priority；注意如图，更改优先级必须是4096整倍数。
![](https://i.postimg.cc/Sxk9rGLR/45-46.png)

```
SW1(config)#spanning-tree vlan 1 priority 40960
SW1(config)#spanning-tree vlan 10 priority 8192
SW1(config)#do show sp
```

![](https://i.postimg.cc/pLYsJHrF/07-17.png)

我们再将vlan1 `spanning-tree vlan 1 root primary`

-->

### 选举RP（Root Port）

除根交换机外，交换机都会选择一个Root Port，Root Port是最接近Root Switch的端口。“最近”按照网口带宽的开销值大小比较。

![](https://i.postimg.cc/qMQjgCBC/49-17.png)

SW2用f0/1要经过如1条100Mb的链路，而用f0/2要经过2条(Cost=19+19=38)，因此SW2选用f0/1 为Root Port，SW3同理。

![](https://i.postimg.cc/ZKC1Nyj2/Snipaste-2019-11-08-20-57-25.png)
