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

首先在拓扑中选举Root Switch，由Bridge ID小的胜出，Bridge ID即优先级和交换机mac地址共同构成的一组数值。默认优先级等同情况下再比较mac地址选出Root Switch。还有个显著的特征根网桥端口全为指定端口。

* Switch Priority = 32768(默认优先值)+vlan id
* 图中bridge id处的address即为交换机的mac地址
* 默认情况下，所有交换机端口都隶属vlan 1

![](https://i.postimg.cc/Y9xB9bpS/11-02.png)

可通过修改交换机vlan1的优先级，让其他交换机成为BP（Root Switch），注意更改优先级必须是4096整倍数。

```
//当前交换机修改vlan id的优先级后的数值比其他交换机数值低后，
//那么它就会成为该vlan id的根网桥
spanning-tree vlan [id] priority [num]
```

![](https://i.postimg.cc/Sxk9rGLR/45-46.png)


### 选举RP（Root Port）

除Bridge Priority胜出的Root Switch，其他交换机都会选择一个RP，根端口是最接近Root Switch的端口，“最近”按网口带宽的开销值大小比较。

![](https://i.postimg.cc/qMQjgCBC/49-17.png)

SW3用f0/1经过SW1需1条100Mb的链路，而用f0/2要经过2条(Cost=19+19=38)，因此SW2选用f0/1为RP根端口，SW2同理。

![](https://i.postimg.cc/RhcPdw1y/40-28.png)

当然，我们可以进入f0/1接口用`spanning-tree vlan [id] cost [number]`来增加cost，致使f0/2成为RP。

![](https://i.postimg.cc/4N6Qnz1H/Snipaste-2019-11-09-23-02-08.png)

### 选举DP(Designated Port)与阻塞非指定端口

在每组相连交换机端口选一个作为指定端口，先比较链路开销cost，如果开销相同，则后比较bridge id（优先级+mac地址）。剩下落选的端口，也就是非指派端口(Non-Designated Port)会被显示为 Altn (Alternate Port) 或 Back (Backup Port)，作为Root Port的备份端口，以提供该交换机从其他交换机的DP到根桥的另一条可切换路径。

![](https://i.postimg.cc/1590mFw3/a19.png)



