最好的 MitM 中间人攻击开源框架清单：https://github.com/Chan9390/Awesome-MitM



kali流量转发：

```bash
cat /proc/sys/net/ipv4/ip_forward 			#值为0表示没开启流量转发，为1表示开启了
echo 1 > /proc/sys/net/ipv4/ip_forward	#开启流量转发
```

开启限速：

```bash
限制网速200ms延时
sudo tc qdisc add dev eth0 root netem delay 200ms
 
取消限制网速200ms延时
sudo tc qdisc del dev eth0 root netem delay 200ms
```



## webspy

使用webspy嗅探受害者访问了哪些网页。
前提步骤同“嗅探图片”，使用arpspoof开启欺骗，然后使用webspy来抓，但是这个工具还是依旧难用，只能抓取一次，然后自己就崩溃了

```bash
webspy -i eth0 指定ip
```





## urlsnarf

使用urlsnarf嗅探受害者访问了哪些网页，默认抓取80、8080、3128端口的数据。
前提步骤同“嗅探图片”，使用arpspoof开启欺骗，然后使用urlsnarf来抓，这个工具好用一些

```bash
urlsnarf -i eth0
```



### dsniff 嗅探账密

当网络中出现了填写账号密码的数据时，dsniff这款工具就开始截获数据，它的-m参数会解析网络协议。
前提步骤同“嗅探图片”，使用arpspoof开启欺骗，然后使用dsniff来抓。
注意，dsniff不会实时显示结果，当你在win7中输入quit的时候dsniff才会显示结果

```
dsniff: 一个密码侦测工具，他能够自动分析端口上收到的某些协议的数据包，并获取相应的密码。dnisff支持的协议有FTP, Telnet, SMTP, HTTP, POP, poppass, NNTP, IMAP, SNMP, LDAP, Rlogin, RIP, OSPF, PPTP MS-CHAP, NFS, VRRP, YP/NIS, SOCKS, X11, CVS, IRC, AIM, ICQ, Napster, PostgreSQL, Meeting Maker, Citrix ICA, Symantec pcAnywhere, NAI Sniffer, Microsoft SMB, Oracle SQL*Net, Sybase and Microsoft SQL;
filesnart: 嗅探网络文件系统（NFS）的流量，并选定某个文件，转储到本地当前工作目录;
mailsnarf: 可以嗅探SMTP和POP流量，并以Berkeley邮件格式输出e-mail消息;
msgsnarf:可以嗅探聊天软件的聊天内容，包括AOL,ICQ 2000, IRC, MSN Messenger, 或Yahoo  Messenger;
urlsnarf: 可以嗅探HTTP请求报文的内容，并以CLF (Common Log Format）通用日志格式输出;
webspy: 指定一个要嗅探的主机，如果指定主机发送HTTP请求，打开网页，webspy也会通过netscape浏览器在本地打开一个相同的网页;
sshmitm: 是Dsniff自带的一个具有威胁的工具之一。首先通过dnsspoof伪造实际机器主机名将攻击目标主机的SSH连接转到本地，那么sshmitm可以截获来自主机的密钥，并获得被劫持连接中的所有信息解码，然后重新转发SSH流量到SSH服务器;
webmitm:与sshmitm类似，也需要dnsspoof的"配合"，不同的是，webmitm"劫持"的是HTTP和HTTPS会话过程，捕获SSL的加密通信;
arpspoof:启用arp欺骗，将自己网卡的IP地址伪装成指定IP地址的MAC;
dnsspoof: 启用DNS欺骗，如果dnsspoof嗅探到局域网内有DNS请求数据包，它会分析其内容，并用伪造的DNS响应包来回复请求者。如果是 请求解析某个域名，dnsspoof会让该域名重新指向另一个IP地址（黑客所控制的主机），如果是反向IP指针解析，dnsspoof也会返回一个伪造的域名;
macof:用来进行MAC flooding，可以用来使交换机的MAC表溢出，对于以后收到的数据包以广播方式发送。注意：在进行MAC泛洪之前就存在于交换机MAC表中的条目不会被覆盖，只能等到这些条目自然老化;
tcpkill: 能够切断指定的TCP会话连接，主要是基于TCP的三次握手过程;
tcpnice: 能够通过在添加活动的流量，降低指定的LAN上的TCP连接的速度.
```





## dnsspoof

dnsspoof启用DNS欺骗，如果是请求解析某个域名，dnsspoof会让该域名重新指向另一个IP地址（黑客所控制的主机），如果dnsspoof嗅探到局域网内有DNS请求数据包，它会分析其内容，并用伪造的DNS响应包来回复请求者。
由于dns查询是客户机向网关发起请求，所以我们如果要在局域网中实现DNS欺骗，仍然是需要先做到arp欺骗。

```bash
参数：
-i 指定一个网卡
-f 指定要欺骗的网址文件，如果不指定，将返回本机的IP地址给被攻击者
```

### 示例：

1：编辑dnsspoof的配置文件

```bash
updatedb	#更新一下locate的数据库
locate dnsspoof.host	#使用locate命令定位文件位置
```

2：kali上开启apache服务，让受害者可以访问到页面

```bash
systemctl start apache2.service
```



3：发起DNS欺骗

```bash
dnsspoof -i eth0 -f /usr/lib/x86_64-linux-gnu/dnsspoof.hosts
```



-i参数指定网卡，-f调用配置文件，然后kali机会监听eth0网卡中发往udp53端口的数据包，注意啊，是监听网络中目标端口是53的数据包，不是本地监听了一个53端口。