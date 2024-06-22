# 路由

基本概念：

- 路由： 跨越从源主机到目标主机的一个互联网络来转发数据包的过程
- 路由器：能够将数据包转发到正确的目的地，并在转发过程中选择最佳路径的设备
- 路由表：在路由器中维护的路由条目，路由器根据路由表做路径选择
- 直连路由：当在路由器上配置了接口的IP地址，并且接口状态为up的时候，路由表中就出现直连路由项
- 静态路由：是由管理员手工配置的，是单向的。
- 默认路由：当路由器在路由表中找不到目标网络的路由条目时，路由器把请求转发到默认路由接口。

## 特点

静态路由

- 路由表是手工设置的；
- 除非网络管理员干预，否则静态路由不会发生变化；
- 路由表的形成不需要占用网络资源；
- 适用环境：一般用于网络规模很小、拓扑结构固定的网络中。

默认路由

- 在所有路由类型中，默认路由的优先级最低
- 适用环境：一般应用在只有一个出口的末端网络中或作为其他路由的补充

浮动静态路由

- 路由表中存在相同目标网络的路由条目时，根据路由条目优先级的高低，将请求转发到相应端口；
- 链路冗余的作用；

## route

```bash
$ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
192.168.1.17    0.0.0.0         255.255.255.255 UH    0      0        0 ppp0
```

参数解释:

- Destination：目标网络或目标主机。
- Gateway：网关地址,`0.0.0.0` 表示当前记录对应的 Destination 跟本机在同一个网段，通信时不需要经过网关
- Genmask：网络掩码，Destination 是主机时需要设置为 `255.255.255.255`，是默认路由时会设置为 `0.0.0.0`

- Flags：标记
  - U：路由是活动的
  - H：表示目标是具体主机，而不是网段
  - N:   表示目标是网段
  - G：路由指向网关
  - R：恢复动态路由产生的表项
  - D：由路由的后台程序动态地安装
  - M：由路由的后台程序修改
  - ！：拒绝路由
- Metric：路由距离，到达指定网络所需的中转数，是大型局域网和广域网设置所必需的 （不在Linux内核中使用。）
- Ref： 路由项引用次数 （不在Linux内核中使用。）
- Use： 此路由项被路由软件查找的次数
- Iface： 网卡名字，例如 `eth0`

## 路由配置

```sh
Usage: inet_route [-vF] del {-host|-net} Target[/prefix] [gw Gw] [metric M] [[dev] If]
       inet_route [-vF] add {-host|-net} Target[/prefix] [gw Gw] [metric M]
                              [netmask N] [mss Mss] [window W] [irtt I]
                              [mod] [dyn] [reinstate] [[dev] If]
       inet_route [-vF] add {-host|-net} Target[/prefix] [metric M] reject
       inet_route [-FC] flush      NOT supported
       
#参数详解
# add           添加一条路由规则
# del           删除一条路由规则
# -net          目的地址是一个网络
# -host         目的地址是一个主机
# target        目的网络或主机
# netmask       目的地址的网络掩码
# gw            路由数据包通过的网关
# dev           为路由指定的网络接口
```

示例：

```bash
# 添加到主机的路由
route add -host [Targetip] name netcard:[Metric]
route add -host 192.168.1.2 dev eth0:0
route add -host 10.20.30.148 gw 10.20.30.40
  
# 添加到网络的路由
route add -net 10.20.30.40 netmask 255.255.255.248 eth0
route add -net 10.20.30.48 netmask 255.255.255.248 gw 10.20.30.41
route add -net 192.168.1.0/24 eth1
  
# 添加默认路由
route add default gw 192.168.1.1
  
#删除路由
route del -host 192.168.1.2 dev eth0:0
route del -host 10.20.30.148 gw 10.20.30.40
route del -net 10.20.30.40 netmask 255.255.255.248 eth0
route del -net 10.20.30.48 netmask 255.255.255.248 gw 10.20.30.41
route del -net 192.168.1.0/24 eth1
route del default gw 192.168.1.1

# 删除所有的默认路由
route del default   
 
# 添加一条默认路由
route add default gw 10.0.0.1      # 默认只在内存中生效
# 开机自启动可以追加到/etc/rc.local文件里
echo "route add default gw 10.0.0.1" >>/etc/rc.local
 
# 添加一条静态路由
route add -net 192.168.2.0/24 gw 192.168.2.254
# 要永久生效的话要这样做：
echo "any net 192.168.2.0/24 gw 192.168.2.254" >>/etc/sysconfig/static-routes
 
# 添加到一台主机的静态路由
route add -host 192.168.2.2 gw 192.168.2.254
# 要永久生效的话要这样做：
echo "any  host 192.168.2.2 gw 192.168.2.254 " >>/etc/sysconfig/static-routes

# 注：Linux 默认没有这个文件 ，得手动创建一个
```

### 静态路由配置

```sh
ip route [destination_network] [mask] [next-hop_address] administrative_distance]

# 参数解析：
# ip route                   用于创建静态路由的命令。
# Destination_network        需要发布到路由表中的网段。
# Mask                       在这一网络上使用的子网掩码。
# Next-hop_address           下一跳路由器的地址。
# administrative_distance    默认时，静态路由有一个取值为1 的管理性距离。在这个命令的尾部添加管理权来修改这个默认值。
 
# 例如
ip route 172.16.1.0 255.255.255.0 172.16.2.1
```

## [#](https://taketo.cc/pages/linux/bash/route/#设置包转发)设置包转发

```sh
# 临时开启路由功能：
echo 1 > /proc/sys/net/ipv4/ip_forward
# 或者
sysctl -w net.ipv4.ip_forward=1

# 永久开启路由功能
vim /etc/sysctl.conf
# 写入以下内容
net.ipv4.ip_forward = 1
```