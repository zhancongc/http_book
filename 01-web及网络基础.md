## 第一章 web及网络基础

[TOC]

### 1.1 使用http访问web

客户端： 发送请求，获取服务器资源的web浏览器。

客户端通过指定的访问地址获取服务器资源，或者上传资源到服务器。

### 1.2 http诞生

欧洲核子研究组织（CERN）的Dr. Tim Berners Lee提出World Wide Web，WWW有三项构建技术：

- Standard Generalized Markup Language --> HyperText Markup Language

- HyperText Transfer Protocol

- Uniform Resource Locator

HTTP协议以TCP/IP协议族为基础，TCP/IP 协议族包含了连个两个设备通信所涉及到的所有软硬件上的规则。

### 1.3 TCP/IP协议

#### 1.3.1 TCP/IP协议分层

TCP/IP协议分为四层：

- 应用层
- 传输层
- 网络层
- 数据链路层

应用层决定了向用户提供应用服务时的通信活动。TCP/IP协议族内预存了各类通用的应用服务，比如File Transfer Protocol、Domain Name System、Hyper Text Transfer Protocol。

传输层提供处于网络连接中的两台设备之间的数据传输。传输层有两个性质不同的协议：Transmission Control Protocol 和 User Data Protocol 

网络层处理网络上流动的数据包，它规定了数据包通过怎样的路径传送到对方计算机。数据包是网络传输的最小数据单位。

数据链路层处理网络连接的硬件部分，包括控制操作系统，硬件设备的驱动，Network Interface Card，光纤等。硬件范畴上的东西都属于数据链路层。

![HTTP数据传输过程](./images/HTTP数据传输过程.bmp)

#### 1.3.2 TCP/IP通信过程

以HTTP通信为例：

- 应用层将HTTP数据传送到传输层；
- 传输层将HTTP报文进行分割，并打上标记序号、端口号，再转发到网络层；
- 网络层增加通信目的地的MAC地址，并转发给数据链路层；
- 数据链路层内将数据包发送到目的地，再向上传送到目的地的应用层。

封装： 数据包向下层传输时，每经过一层都会加上该层所属的头部信息。

### 1.4 HTTP相关的协议：IP、TCP、DNS

#### 1.4.1 IP协议

IP协议：Internet Protocol 网络层，将source的数据包发送给destination。最重要的两个依据是：IP地址和MAC（Media Access Control Address）地址。

IP地址是网络节点被分配到的地址，MAC地址是网卡所属的固定地址。IP地址会更改，但是MAC地址不会更改。

ARP协议：网络中转时，使用ARP（Address Resolution Protocol）协议，通过MAC地址来查找目标的IP地址。

#### 1.4.2 可靠的TCP协议

 TCP协议在传输层，提供可靠的字节流服务：

- 可靠意味着：TCP协议可以确认数据是否送达。
- 字节流服务（Byte Stream Service）：为了方便传输，将大块数据分割成以报文段为单位的数据包，方便传输和管理。

为了可靠地传输，TCP协议采用了三次握手的策略（Three-way handshaking）。

首先，发送端将带有SYN标识的数据包发送给接收方；然后，接收端回传一个带有SYN/ACK标识的数据包作为确认；最后，发送端发送一个带有ACK标识的数据包，代表握手结束。

如果握手过程莫名中断，TCP协议下，发送端会再次以相同的顺序发送相同的数据包。


#### 1.4.3 DNS服务

DNS协议和HTTP协议一样都在应用层。DNS提供域名和IP的双向查找服务。

![浏览器请求服务器资源过程](./images/浏览器请求服务器资源过程.bmp)

### 1.5 URI

URI（Uniform Resource Identifier），标识了网络上所有的资源，支持多种协议： http， ftp， telnet， file， mailto等。

URL（Uniform Resource Locator），表明网络资源的位置，是URI的子集。

绝对URI的格式：

```php
http://user:password@www.example.com:80/dir/index.html?uid=1#ch1
```

格式说明：

| 字段              | 说明                                   |
| ----------------- | -------------------------------------- |
| `http`            | 协议名称（可以是程序或者脚本名称）     |
| `user:password`   | 登录信息                               |
| `www.example.com` | 服务器地址（可以是域名也可以是IP地址） |
| `80`              | 端口号（省略则使用默认端口号）         |
| `/dir/index.html` | 带层次的文件路径                       |
| `uid=1`           | 查询字符串                             |
| `#ch1`            | 片段标识符                             |