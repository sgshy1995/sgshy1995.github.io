# 关于 URL


# 万维网/WWW

[万维网](https://zh.wikipedia.org/wiki/%E4%B8%87%E7%BB%B4%E7%BD%91)（World Wide Web）亦作 WWW、Web，是一个透过互联网访问的，由许多互相链接的超文本组成的系统。

***WWW = HTTP + URI + HTML***

# URL

URI（Uniform Resource Identifier，缩写：URI）是一个全球网络资源唯一认证的系统，称为统一资源标识符。

URI 的最常见的形式是 **统一资源定位符**（URL），经常指定为非正式的网址。

[统一资源定位符](https://zh.wikipedia.org/wiki/%E7%BB%9F%E4%B8%80%E8%B5%84%E6%BA%90%E5%AE%9A%E4%BD%8D%E7%AC%A6)（Uniform Resource Locator，缩写：URL；或称统一资源定位器、定位地址、URL地址，俗称网页地址或简称网址）是因特网上标准的资源的地址（Address），如同在网络上的门牌。

它最初是由[蒂姆·伯纳斯-李](https://zh.wikipedia.org/wiki/%E8%92%82%E5%A7%86%C2%B7%E4%BC%AF%E7%BA%B3%E6%96%AF-%E6%9D%8E)发明用来作为万维网的地址，现在它已经被万维网联盟编制为因特网标准RFC 1738。

大致分为几部分：

## 传送协议

网页广泛使用的协议基本只有两种： HTTP 和 HTTPS 。

### HTTP

[超文本传输协议](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)（HyperText Transfer Protocol）是一种用于分布式、协作式和超媒体信息系统的应用层协议。HTTP是万维网的数据通信的基础。

### HTTPS

[超文本传输安全协议](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%AE%89%E5%85%A8%E5%8D%8F%E8%AE%AE)（HyperText Transfer Protocol Secure；常称为HTTP over TLS、HTTP over SSL 或 HTTP Secure）是一种通过计算机网络进行安全通信的传输协议。

HTTPS经由HTTP进行通信，但利用SSL/TLS来加密数据包。HTTPS开发的主要目的，是提供对网站服务器的身份认证，保护交换数据的隐私与完整性。

### TCP/IP

HTTP 协议的底层其实是由 TCP 协议和 IP 协议（简称 TCP/IP）构建的。

#### TCP 和 UDP 的区别是什么

TCP 可靠、面向连接、相对 UDP 较慢；UDP 不可靠，不面向连接、相对 TCP 较快。

#### TCP 的三次握手指的是什么

每次建立连接前，客户端和服务端之前都要先进行三次对话才开始正式传输内容，三次对话大概是这样的：

* 客户端：我要连接你了，可以吗
* 服务端：嗯，我准备好了，连接我吧
* 客户端：那我连接你咯。

## 服务器

通常为域名，也可为IP地址。

### IP 地址

[互联网协议地址](https://zh.wikipedia.org/wiki/IP%E5%9C%B0%E5%9D%80)（Internet Protocol Address，又译为网际协议地址），缩写为IP地址（IP Address），是分配给网络上使用网际协议（Internet Protocol, IP）的设备的数字标签。

IP地址主要有两个功能：标识主机或者网络和寻址。

常见的 IP 地址分为 IPv4 与 IPv6 两大类，但是也有其他不常用的小分类。***IP 地址是唯一的***。

IP 地址由 32位 二进制数组成，为便于使用，常以 XXX.XXX.XXX.XXX 形式表现，每组 XXX 代表小于或等于 255 的 10进制数，该表示方法称为点分十进制。

### 域名

[网域名称](https://zh.wikipedia.org/wiki/%E5%9F%9F%E5%90%8D)（Domain Name，简称：Domain），简称域名、网域，是由一串用点分隔的字符组成的互联网上某一台计算机或计算机组的名称，用于在数据传输时标识计算机的电子方位。

域名可以说是一个IP地址的代称，目的是为了便于记忆后者。例如，baidu.com是一个域名，和 IP 地址 220.181.38.148 相对应。人们可以直接访问 baidu.com 来代替 IP 地址，然后域名系统（DNS）就会将它转化成便于机器识别的IP地址。这样，人们只需要记忆这一串带有特殊含义的字符，而不需要记忆没有含义的数字。

- 一个域名可以对应不同的 IP（负载均衡）
- 一个 IP 也可以对应不同的域名（共享主机）

### DNS

域名的核心是[域名系统](https://zh.wikipedia.org/wiki/%E5%9F%9F%E5%90%8D%E7%B3%BB%E7%BB%9F)（Domain Name System，缩写：DNS）。

域名系统是互联网的一项服务。*它作为将域名和IP地址相互映射的一个分布式数据库，能够使人更方便地访问互联网。*

DNS 使用 TCP和UDP 端口53。当前，对于每一级域名长度的限制是63个字符，域名总长度则不能超过253个字符。

### nslookup

[nslookup](https://en.wikipedia.org/wiki/Nslookup) 命令用于查询 DNS 的记录，查看域名解析是否正常，在网络故障的时候用来诊断网络问题。

使用最多的方法是直接查询一个域名的 A记录。

```bash
$ nslookup domain [dns-server]
```

如果没有指定 dns-server，用系统默认的 dns 服务器。

```bash
$ nslookup baidu.com

# Server:	180.168.255.118
# Address:	180.168.255.118#53

# Non-authoritative answer:
# Name:	baidu.com
# Address: 220.181.38.148
# Name:	baidu.com
# Address: 39.156.69.79
```

#### 域名级

域名系统中的任何名称都是域名。在域名系统的层次结构中，各种域名都隶属于域名系统根域的下级。

- 域名的第一级是顶级域，它包括通用顶级域，例如.com、.net和.org；以及国家和地区顶级域，例如.us、.cn和.tk。

- 顶级域名下一层是二级域名，一级一级地往下。

- 这些域名向人们提供注册服务，人们可以用它创建公开的互联网资源或运行网站。

- 顶级域名的管理服务由对应的域名注册管理机构（域名注册局）负责，注册服务通常由域名注册商负责。

### ping

[ping](https://zh.wikipedia.org/wiki/Ping)（呯）是一种计算机网络工具，用来测试数据包能否透过IP协议到达特定主机。

ping 的运作原理是向目标主机传出一个 ICMP 的请求回显数据包，并等待接收回显回应数据包。程序会按时间和成功响应的次数估算丢失数据包率（丢包率）和数据包往返时间（网络时延）。

这里展示 Linux/类Unix 系统中的 ping:

```bash
$ ping baidu.com

# PING baidu.com (220.181.38.148): 56 data bytes
# 64 bytes from 220.181.38.148: icmp_seq=0 ttl=51 time=27.892 ms
# 64 bytes from 220.181.38.148: icmp_seq=1 ttl=51 time=29.555 ms
# 64 bytes from 220.181.38.148: icmp_seq=2 ttl=51 time=29.480 ms
# 64 bytes from 220.181.38.148: icmp_seq=3 ttl=51 time=29.192 ms
# 64 bytes from 220.181.38.148: icmp_seq=4 ttl=51 time=29.236 ms
# ...
# ^C
# --- baidu.com ping statistics ---
# 11 packets transmitted, 11 packets received, 0.0% packet loss
# round-trip min/avg/max/stddev = 27.892/29.640/32.078/0.949 ms
```

- 按 control/ctrl+C 中断。总计返回11个数据包，0丢包，最小27.892ms，最大29.640ms。

- IP 地址是 220.181.38.148。

- windows 系统下会默认只返回4个数据包，以32 bytes 大小测试。

## 端口

[端口](https://zh.wikipedia.org/wiki/%E9%80%9A%E8%A8%8A%E5%9F%A0)（port），又称为连接端口、协议端口（protocol port）。

每个端口都会与主机的IP地址及通信协议关联。端口以16比特数字来表示，这被称为端口编号（port number）。

- 一共有 65535 个端口
- 0--1023 系统保留，只能由root用户使用。
- 其他端口给普通用户使用，端口可能存在被占用情况。

### 一些默认端口

- 21: 文件传输协议 FTP
- 80: 超文本传输协议 HTTP
- 443: 超文本传输安全协议 HTTPS
- 156: SQL 服务
- 170: 打印服务
- 53: 域名服务系统 DNS
- 513: 路由器

## 路径

路径用于请求一个IP或域名下的不同页面。

以“/”字符区别路径中的每一个目录名称。

## 查询参数

查询参数可以查看同一个页面下的不同内容。

以 “?” 字符为起点，每个参数以 “&” 隔开，再以 “=” 分开参数名称与数据，通常以 UTF8 的 URL 编码，避开字符冲突的问题。

## 锚点

也称片段。用于查看页面中同一内容的不同位置。

以“#”字符为起点。

## 示例

```
https://www.baidu.com:443/s?wd=vue#5
```

- 协议为 https。大多数网页浏览器不要求用户输入网页中 “https://” 的部分，因为绝大多数网页内容是超文本传输/安全传输协议文件
- 域名为 www. baidu. com
- 端口为 443。常用端口号也不必书写
- 路径为 /s
- 查询参数为 ?wd=vue
- 锚点为 #5

![url](/images/http/7.png)