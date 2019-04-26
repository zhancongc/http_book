# 图解HTTP协议摘要

> Author: zhancc
> Date: 2018-10-08

## 目录

[TOC]

## 第一章 web及网络基础

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

[Back](#目录)

## 第二章 简单的HTTP协议

### 2.1 客户端和服务端之间的通信

HTTP协议是基于客户端和服务端之间的通信

### 2.2 通过请求和响应的交换达成通信

请求必定是从客户端发出，服务端响应请求并返回。HTTP协议不支持双向通信，WebSocket协议支持。

客户端请求报文：

```
POST /form/entry HTTP/1.1
Host: example.com
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 16
name=ueno&age=37
```

服务端响应报文：

```
HTTP/1.1 200 OK
Date: Tue， 10 Jul 2012 06:50:15 GMT
Content-Length: 362
Content-Type: text/html

<html>
...
```

### 2.3 HTTP是不保存状态的协议

HTTP协议自身不保存请求和相应之间的通信状态。可以使用cookie来保存状态信息。

### 2.4 请求URI定位资源

URI可以定位互联网上的所有资源。

```
GET /index.html HTTP/1.1
Host: example.com
```

### 2.5 与服务端交互的方法

- GET：访问已经被URI识别的资源，指定的资源被解析之后，返回响应的内容。

```
GET /index.html HTTP/1.1
Host: example.com
-------------------------
返回index.html的页面资源

GET /index.html HTTP/1.1
Host: example.com
If-Modified-Since: Thu, 12 Jul 2012 07:30:00 GMT
-------------------------------------------------------
仅返回2012年7月12日07:30:00以后更新过的index.html的页面资源
如果没有内容更新，返回状态码 304 Not Modified
```

- POST：传输客户端的数据

```
POST /submit.cgi HTTP/1.1
Host: www.example.com
Content-Length: 1560
-----------------------------
返回submit.cgi接收数据的处理结果
```

- PUT：指定URI上传文件，自身没有验证机制，一般不开放。REpresentational State Transfer标准的Web网站可能会开放。

```
PUT /example.html HTTP/1.1
Host: www.example.com
Content-Type: text/html
Content-Length: 1560
-----------------------------------------
返回状态码 204 No Content（表示该文件已存在）
```

- HEAD：除了不返回报文主体部分，其它和GET方法一样。用于确认URI的有效性和资源的更新时间。

```
HEAD /example.com HTTP/1.1
Host: www.example.com
--------------------------
返回index.html的响应首部
```

- DELETE：删除指定URI上的文件

```
DELETE /example.com HTTP/1.1
Host: www.example.com
---------------------------------------------
返回状态码 204 No Content（表示该html已经被删除）
```

- OPTIONS： 返回针对URI指定资源支持的方法

```
OPTIONS /example.com HTTP/1.1
Host: www.example.com
--------------------------------------------------
HTTP/1.1 200 OK
Allow: GET, POST, HEAD, OPTIONS(返回服务器指定的方法)
```

- TRACE：追踪路径，因为Cross-Site Tracing Attack，基本不用

- CONNECT： 要求使用隧道协议连接代理，在隧道内使用TCP协议通信

  使用 Secure Socket Layer 和 Transport Layer Security 加密传输

  ```
  CONNECT proxy.example.com:8080 HTTP/1.1
  Host: proxy.example.com
  -----------------------------------------
  HTTP/1.1 200 OK(进入网络隧道)
  ```

### 2.6 HTTP/1.1支持的方法

| Method  | Explanation          |
| ------- | -------------------- |
| GET     | 获取资源             |
| POST    | 传输实体主体         |
| PUT     | 传输文件             |
| HEAD    | 获取报文头部         |
| DELETE  | 删除文件             |
| OPTIONS | 询问支持的方法       |
| TRACE   | 追踪路径             |
| CONNECT | 使用隧道协议连接代理 |

### 2.7 持久连接节省通信量

  在HTTP最初的设计中，每进行一次HTTP通信都要断开一次TCP连接。

  ![客户端和服务端的HTTP通信过程](./images/客户端和服务端的HTTP通信过程.bmp)

  每个资源都要发起一次完整的HTTP请求，一个网页能需要很多次请求。

  ![一个网页多次HTTP请求](./images/一个网页多次HTTP请求.bmp)

  持久连接：Keep-Live，只要任意一端没有明确提出断开连接，则一直保持TCP连接状态。

  ![一个网页一次HTTP请求](./images/一个网页一次HTTP请求.bmp)

  持久连接的好处是：减少了TCP连接重复地建立和断开造成的额外开销，减轻了服务端的负载。并且降低了请求和响应时间，提高了web页面的响应速度。

管线化：不用等待服务端响应，就可以直接发送下一个请求。请求越多，响应速度提升越明显。

![管线化请求](./images/管线化请求.bmp)

### 2.8 cookie

HTTP协议是无状态协议，不记录之前发生过的请求和响应状态。

Cookie技术，服务端在响应报文中添加一个Set-Cookie的首部字段信息，通知客户端保存Cookie。当下次客户端在往服务端发送请求时，客户端会自动在请求头部添加cookie后发送。服务端根据cookie查询连接记录，找到对应的连接状态。

![带cookie的HTTP请求](./images/带cookie的HTTP请求.bmp)

```
GET /reader/ HTTP/1.1
Host www.example.com
---------------------------------------------------------------------------------
HTTP/1.1 200 OK
Date: Thu， 12 Jul 2012 07:12:20 GMT
Server: Apache
<Set-Cookie: sid=1342077140226724; path=/; expires=Wed, 10-Otc-12 07:12:20 GMT>
Content-Type: text/plain; charset: UTF-8
---------------------------------------------------------------------------------
GET /image/ HTTP/1.1
Host: www.example.com
Cookie: sid=1342077140226724
```

[Back](#目录)

## 第三章 HTTP报文内的信息

### 3.1 HTTP报文

客户端发送的是请求报文，服务端发送的是响应报文。

HTTP报文有多行数据构成的字符串文本，以CR+LF作为空行分割报文头部和报文主体。

Carriage Return 回车 0x0d；Line Feed 换行 0x0a。

![请求报文和响应报文](./images/请求报文和响应报文.bmp)

请求行：包含请求URI和HTTP版本

状态行：响应结果的状态码，原因短语和HTTP版本

首部字段：请求响应的各种条件和属性，通用首部、请求首部、响应首部、实体首部

### 3.2 编码提升传输速率

#### 3.2.1 报文实体

通过编码提升传输速率，但是会消耗更多的CPU资源。

HTTP报文：由8位组字节流组成，通过HTTP通信传输

实体：作为请求和响应的有效载荷数据传输

通常，报文主体和实体主体。只有在传输过程中发生编码操作时，实体主体的内容发生变化，才导致它和报文主体的内容产生差异。

#### 3.2.2 内容编码

HTTP的内容编码，指明编码格式，将实体信息压缩。最后，由客户端接收并解码。

常见的内容编码有：

- gzip （GNU Zip）
- compress (UNIX标准压缩)
- deflate(zlib)
- identity (没有编码)

#### 3.2.3 分块传输编码

传送大容量数据时，将数据分割成多块，能够让浏览器逐步显示页面，这就是分块传输编码。（Chunked Transfer Coding）

### 3.3 发送多种数据的多部分对象集合

MIME Multipurpose Internet Mail Extension 多用途互联网邮件扩展

它允许邮件处理多种不同类型的数据：文本、图片、视频等

HTTP也支持类似的协议：

multipart/form-data web表单

```
Content-Type: multipart/form-data; boudary=AaB03x

--AaB03x
Content-Disposition: form-data; name="field1"

--AaB03x
Content-Disposition: form-data; name="pics"; filename="file1.txt"
Content-Type: text/plain
/* file1.txt 的数据 */
```

multipart/byteranges 包含多个范围请求 206 Partial Content

> 要实现该功能需要制定下载的实体范围，这种制定范围发送请求叫做范围请求。


针对范围请求，服务器会返回状态码为206的报文，多重范围请求 响应会在头部 Content-Type 表明 multipart-byteranges 返回报文主体。如果服务端不支持范围请求则返回状态码200 OK并将所有报文一并返回。执行范围时会使用头部字段 Range 来指定资源 byte 的范围。

```
HTTP/1.1 200 OK
Date: Thu， 12 Jul 2012 07:12:20 GMT
Content-Type: multipart/byteranges; boundary=THIS_STRING_SEPARATES

--THIS_STRING_SEPARATES
Content-Type: application/pdf
Content-Range: bytes 500-999/8000
/* 范围数据 */
--THIS_STRING_SEPARATES
Content-Type: application/pdf
Content-Range: bytes 7000-7999/8000
/* 范围数据 */
```

### 3.4 范围请求

可以从下载的中断处恢复下载，而不是从头开始下载。实现这种功能需要指定下载的实体范围，也就是范围请求。

![范围请求](./images/范围请求.bmp)

```
指定字节范围：
/* 5001-10000之间 */
Range: bytes=5001-10000
/* 5001以后的全部 */
Range: bytes=5001- 
/* 3000和5000-7000 */
Range: bytes=3000,5000-7000
```

如果服务端无法响应范围请求，则会返回状态码200和完整的实体内容。

### 3.5 返回最合适的内容

同一个页面可能有多个不同的版本，服务端根据实际情况返回最合适的页面。

当浏览器的默认语言是英文，则访问同一个URI返回英文的页面，而不是中文的页面。

内容协商：客户端对服务端返回的内容进行交涉，然后服务端提供最合适的资源。会有语言、字符集、编码方式的不同。

内容协商的三种方式：

- 服务端驱动协商（Server Driven Negotiation）

协商根据首部字段： Accept、Accept-Charset、Accept-Encoding、Accept-Language、Content-Language

- 客户端驱动协商（Agent Driven Negotiation）

由客户端的可选项列表里自动选择，PC版或者mobile版

- 透明协商（Transparent Negotiation）

服务端和客户端都有协商

[Back](#目录)

## 第四章 HTTP状态码

### 4.1 状态码

状态码，就是HTTTP通信状态的数字表示。

| Status Code | Type          | Explanation              |
| :---------- | ------------- | ------------------------ |
| 1XX         | Informational | 接收的请求正在处理       |
| 2XX         | Success       | 请求正常处理完毕         |
| 3XX         | Redirection   | 需要进行附加操作完成请求 |
| 4XX         | Client Error  | 服务器无法完成请求       |
| 5XX         | Server Error  | 服务器处理请求出错       |

### 4.2 Success

| Status Code | Keyword         | Explanation              |
| ----------- | --------------- | ------------------------ |
| 200         | OK              | 成功处理请求并返回       |
| 204         | No Content      | 成功处理，但是无实体返回 |
| 206         | Partial Content | 成功处理，返回部分内容   |

### 4.3 Redirection

| Status Code | Keyword            | Expanation                                   |
| ----------- | ------------------ | -------------------------------------------- |
| 301         | Moved Permanently  | 永久重定向                                   |
| 302         | Found              | 临时性重定向，URI未来还会改变                |
| 303         | See Other          | 该资源由另外一个URI，应该使用GET方法获取资源 |
| 304         | Not Modified       | 资源已找到，但是不满足条件                   |
| 305         | Temporary Redirect | 临时重定向，不会从POST变GET                  |

### 4.4 Client Error

| Status Code | Keyword      | Explanation                                         |
| ----------- | ------------ | --------------------------------------------------- |
| 400         | Bad Request  | 请求报文存在语法错误                                |
| 401         | Unauthorized | 请求需要认证信息，如果是第二次返回401，表示认证失败 |
| 403         | Forbidden    | 请求被服务器拒绝                                    |
| 404         | Not Found    | 服务端找不到请求的资源                              |

### 4.5 Server Error

| Status Code | Keyword               | Explanation                          |
| ----------- | --------------------- | ------------------------------------ |
| 500         | Internal Server Error | 服务端在处理请求时出现了故障         |
| 503         | Service Unavailable   | 服务端无法处理请求，超负载或停机维护 |

如果能够知道服务恢复时间，最好加上 Retry-After 首部字段。

[Back](#目录)

## 第五章 与HTTP相关的Web Server

### 5.1 Virtual Host

HTTP/1.1允许一台主机搭建多个web server，多个域名映射的是同一个ip。

### 5.2 通信数据转发

#### 5.2.1 代理

代理：服务端和客户端的中间人程序，客户端请求和服务端响应的转发者

![代理](./images/代理.bmp)

![URI访问控制](./images/URI访问控制.bmp)

- 缓存代理：预先将资源的副本保存在代理服务器上，当代理再次收到对相同资源请求时，就可以返回缓存的资源副本。

- 透明代理：不对报文做任何修改的代理。

#### 5.2.2 网关

网关：转发其它服务端数据的服务器，能够接收和处理客户端的请求

![网关](./images/网关.bmp)

网关可以提供非HTTP服务，比如：数据库服务等，可以提高服务端的安全性。

#### 5.2.3 隧道

隧道：相隔甚远的客户端和服务端之间的中转，并保持双向连接的应用程序

![隧道通信](./images/隧道通信.bmp)

确保通信的安全性，隧道本身不会解析HTTP请求，它只是报文传递的中转。

### 5.3 保存资源的缓存

缓存：代理服务器或客户端本地磁盘内保存的资源副本。

有了缓存，就可以减少对服务端资源的访问次数，节省了通信流量。

![缓存服务器](./images/缓存服务器.bmp)

缓存的资源也会过期，一般会设定有效期。另外，即使存在缓存，缓存服务器也会向服务器请求，确认资源的有效性。若判断缓存失效，缓存服务器会重新获取新的缓存。

![缓存过期](./images/缓存过期.bmp)

客户端也有缓存，机制和缓存服务器类似。

[Back](#目录)

## 第六章 HTTP首部

### 6.1 报文首部

![请求报文](./images/请求报文.bmp)

![响应报文](./images/响应报文.bmp)

```
GET / HTTP/1.1
Host: hacker.jp
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
----------------------------------------------------------------------------------------------
HTTP/1.1 302 Found
Date: Mon, 15 Oct 2018 06:07:31 GMT
Server: Apache
Location: http://hacker.jp/cgi/index.cgi
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 248
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=iso-8859-1
```

### 6.2 HTTP首部字段

首部字段传递了报文相关的重要信息，有：报文主体大小，所使用的语言，认证信息等。

首部字段由字段名、冒号和字段值组成，一个字段名可能有多个值。

- [通用首部字段](#1.1 通用首部字段)

请求和响应报文都用到的字段

- [请求首部字段](#1.2 请求首部字段)

请求报文，补充了请求的附加内容、客户端信息、响应内容、相关优先级

- [响应首部字段](#1.3 响应首部字段)

响应报文，补充了响应的附加内容

- [实体首部字段](#1.4 实体首部字段)

资源实体，补充了资源的信息，大小、更新时间等



根据缓存代理和非缓存代理行为，HTTP首部分成两类：

- 端到端首部（End-to-End Header）

这类首部会转发到最终接收的目标，而且必须保存在由缓存生成的响应中，而且必须要被转发

- 逐跳首部（Hop-by-Hop Header）

这类首部只对单词转发有效，会因为通过缓存和代理而不再转发。如果使用逐跳首部，需要提供Connection首部字段。

逐跳首部字段有： Connection，Keep-Alive，Proxy-Authenticate，Proxy-Authorization，Trailer，TE，Transfer-Encoding，Upgrade

[Back](#目录)

## 第七章 更安全的HTTPS

### 7.1 HTTP的缺点

- 通信数据明文，内容容易被窃听
- 不验证通信方身份，可能会遭遇伪装
- 无法证明报文的完整性，内容可能会被篡改

#### 7.1.1 窃听

TCP/IP协议族的每一个环节，都会遭到窃听。未加密，则窃听到明文；加密，则窃听到密文。

![报文被窃听](./images/报文被窃听.bmp)

抓包和嗅探工具，Wireshark

解决方案，使用SSL（Secure Socket Layer）或者TLS（Transport Layer Security）建立安全通信线路，这就是HTTPS。

![安全的通信线路](./images/安全的通信线路.bmp)

内容加密并不能保证安全，因为密文也是有被篡改和破译的可能。

#### 7.1.2 伪装

HTTP协议不验证请求方的身份，只要请求，都会返回一个响应。同样地，也不验证URI真正指向的主机。产生了三个隐患：

- 无法确定请求发送至目标的Web服务器是否为按真实意图返回响应的那台服务器。伪装服务器
- 无法确定响应返回到的客户端是否为按真实意图接收响应的那个客户端。伪装客户端
- 无法确定请求方是否具备访问的权限。伪装高级权限用户
- 无法判定请求来自何方
- 即使无意义的请求，也是照单全收，来者不拒。Denied of Service攻击

SSL提供了数字证书，可以用于确定通信双方。证书由值得信任的第三方颁发，用来证明服务器和客户端是实际存在的。

![证书确认服务端身份](./images/证书确认服务端身份.bmp)

#### 7.1.3 篡改

HTTP协议并不能证明报文的完整性，在报文传输的过程中，报文内容可能遭到第三方的篡改。HTTP协议无法判断，发出的报文和接收的报文是相同的。中间人攻击（Man-in-the-Middle attack）

![中间人攻击](./images/中间人攻击.bmp)

虽然HTTP协议有确认报文完整性的方法，但是并不可靠。网站可能会使用PGP（Pretty Good Privacy）和MD5校验，但是也不能百分之百保证数据的完整性。

### 7.2 HTTPS

HTTPS = HTTP + 通信加密 + 证书认证 + 完整性保护

#### 7.2.1 HTTPS是身披SSL外壳的HTTP

HTTPS：HTTP先和SSL层通信，SSL再和TCP通信。

![SSL层](./images/SSL层.bmp)

#### 7.2.2 加密方式

对称密钥加密（共享密钥加密），加密和解密都是同一个密钥。

![对称密钥加密](./images/对称密钥加密.bmp)

对称密钥加密方式的弊端：

- 不能安全地将密钥告诉接收者
- 攻击者拿到密钥就能破译
- 双方都要确保密钥的安全

非对称加密（公开密钥加密），发送方使用公钥加密，接收方使用私钥解密。私钥只有自己知道，公钥任何人都可以获取。

![非对称密钥加密](./images/非对称密钥加密.bmp)

非对称加密，不需要发送解密的密钥，安全性很高，但是解密运算量较大。

#### 7.2.3 HTTP使用混合加密方式

非对称密钥加密更安全，加密解密速度慢；对称加密传输密钥不方便，加密解密速度快。

HTTPS两种加密方式混用：在交换密钥环节使用非对称加密，通信建立之后，在报文交换环节使用对称加密。

![混合加密](./images/混合加密.bmp)

#### 7.2.4 如何证明公钥的正确

非对称加密，无法避免公钥被篡改。解决方案是，使用可信任的第三方机构提供的公钥证书。

具体流程是：

1. 服务端向证书认证机构申请公开密钥；
2. 证书认证机构验证身份之后，对申请的公钥做数字签名证书，然后将证书和公钥绑定在一起发给服务端
3. 服务端将该证书发给客户端，尝试进行加密通信
4. 客户端根据内部植有的证书认证机构的数字签名证书，向证书认证机构发起加密通信，求证服务端的证书
5. 证书认证机构确认证书可信
6. 客户端使用可信的公钥加密数据，和服务端通信

![加密通信建立过程](./images/加密通信建立过程.bmp)

#### 7.2.5 SSL证书

- 可证明组织真实性的EV SSL证书（Extended Validation SSL Certificate）

拥有EV SSL证书， 浏览器的状态栏不仅有绿色的锁标记，还有证书所属组织名称。

- 客户端证书

客户端证书和服务端证书的作用是类似的，它是为了验证客户端的身份。每个客户端都需要安装，意味着巨大的安装成本。另外，客户端证书只是验证了客户端的真实存在，并不能验证使用者身份的真实有效。那么，安装客户端证书的人，就拥有了客户端证书的使用权限。

这些缺陷限制了客户端证书的使用范围，只有少数对安全性要求极高的业务才会使用客户端证书，如：网上银行。

- 自签名证书

自签名证书由于没有获取第三方证书认证机构的认证，无法消除伪装的可能性，浏览器并不认可，并且会提示网站有安全风险。有些中级签名证书，也会被浏览器判定存在风险。

### 7.3 HTTPS通信

#### 7.3.1 通信过程

```sequence
Note right of Client: Connection Start
Client->Server: (1)HandShake: ClientHello
Server-->Client: (2)HandShake: ServerHello
Server-->Client: (3)HandShake: Certificate
Server-->Client: (4)HandShake: ServerHelloDone
Client->Server: (5)HandShake: ClientKeyExchange
Client->Server: (6)ChangeCipherSpec
Client->Server: (7)HandShake: Finished
Server-->Client: (8)ChangeCipherSpec
Server-->Client: (9)HandShake: Finished
Note right of Client: Connection Established
Client->Server: (10)Application Data via HTTP
Server-->Client: (11)Application Data via HTTP
Note right of Client: Connection close
Client->Server: (12)Alert: Warning, close notify
```

ClientKeyExchange以服务端发来的公钥加密， 传输一个Pre-Master secret的随机密码串。

ChangeCipherSpec报文告诉服务端，接下来的报文会用Pre-Mater secret为密钥加密。

Finished报文包含连接开始至今全部报文的校验值，客户端和服务端的Finished报文交换完毕，SSL才算建立完成。

应用层发送数据时，会附带MAC（Message Authentication Code）的报文摘要，如果报文被篡改，MAC会改变。

#### 7.3.2 SSL速度慢

使用SSL速度变慢有两个方面的原因：

- 数据传输慢，额外的数据包
- 加密解密运算，额外的运算开销

![SSL速度慢](./images/SSL速度慢.bmp)

[Back](#目录)

## 第八章 用户访问认证

### 8.1 何为认证

计算机验证用户的身份的凭据有：

- 密码：注册账户时，提交的用户验证身份的字符串
- 动态令牌：本人持有的设备上才有的一次性密码
- 数字证书：用户持有的终端携带的身份证明
- 生物认证：指纹、虹膜、面部
- IC：带有数字芯片的卡片

对于计算机而言，只要能通过验证，就认为是本人。因此，验证信息绝不能泄露。

HTTP所使用的认证方式有：

- BASIC认证，基本认证
- DIGEST认证，摘要认证
- SSL客户端认证
- FormBase认证，表单认证

### 8.2 BASIC认证

```sequence
Client->Server: GET / HTTP/1.1<CR> Host: example.com
Note left of Server: XXX: input your id and password
Server-->Client: HTTP/1.1 401 Authorization Required<CR> WWW-Authenticate: Basic realm="XXX"
Client->Server: GET / HTTP/1.1<CR> Host: example.com<CR> Authorization: Basic YYY
Note right of Client: YYY: id and password
Server-->Client: HTTP/1.1 200 OK
```

![basic认证](./images/basic认证.bmp)

Basic认证明文传输密码，安全度几乎为零 。另外Basic认证只有登录，没有注销。所以目前，几乎没有网站使用这种认证。

### 8.3 DIGEST认证

DIGEST使用质询响应的方式，没有传输明文密码。

质询响应：认证方发送质询码，接收方计算出响应码返回。

![digest认证](./images/digest认证.bmp)

WWW-Authenticate首部字段必须包含realm和nonce值，nonce是随机字符串，realm是Response-URI安全域字符串。

Authorization：必须包含username、realm、nonce、uri和response字段信息。response是密码的MD5值，username是realm限定范围内的用户名。uri就是Request-URI。

认证成功后，Authentication-Info会写入认证成功的相关信息。

### 8.4 SSL客户端认证

凭借SSL客户端认证，服务端可以确认客户端是否可信。

1. 客户端安装SSL证书
2. 客户端请求资源，服务器发送Certificate Request报文，要求客户端提供客户端证书
3. 客户端发送客户端证书，Client Certificate报文
4. 服务端验证证书是否可信，取出可信证书的公钥，进行HTTPS通信

实际上，采用SSL客户端认证的情况下，还要求认证者提供其它信息，如密码、口令等。既保证客户端可信，又确保了用户可信。

客户端认证需要支付的费用有：购买证书的费用，维持证书运营的费用。

### 8.5 表单认证

用户以表单的形式发送用户id和密码等登录信息到服务端的认证方式。

目前，最常见的认证还是表单认证。Baisc、Digest认证安全性不高，SSL客户端认证成本很高。

表单认证会使用Cookie来管理Session。因为HTTP是无状态的协议，认证过的客户端，再发起请求时，服务端并不能辨别。使用Cookie就可以实现状态管理。

![cookie管理session](./images/cookie管理session.bmp)

cookie中的SessionID是用户的标志，如果SessionID被盗，第三方就可以伪装成已认证用户进行操作。

另外，服务端也不能明文保存用户密码，一种安全的做法是：先给密码增加额外信息(salt)，然后计算其hash函数算出散列值保存。

>salt是服务器随机生成的字符串，长度足够长，并且是真正随机生成的，然后将其和密码字符串相连接生成散列值。当两个用户使用了同一个密码时，也会因为salt值不同，生成的散列值也不同。这样就可以减少密码特征，攻击者很难用密码特征库进行破解。

[Back](#目录)

## 第九章 基于HTTP功能的追加协议

HTTP在功能和性能上已经不能满足现在的需求。

### 9.1消除HTTP瓶颈的spdy

SPDY没有改写HTTP协议，只是在TCP/IP的应用层和传输层之间添加一个会话层。为了确保安全，SPDY规定通信过程必须使用SSL。

使用SPDY之后，HTTP协议额外获取了：


- 多路复用： 所有的HTTP请求都使用同一个TCP连接
- 并发请求：请求可以无限并发，并且可以规定优先级
- 压缩HTTP首部： 压缩HTTP的首部，传递同样的数据，报文数减少了
- 推送功能： 支持服务端主动向客户端推送数据，而不必等待客户端的请求
- 服务器提示功能：主动提示客户端可以获取的资源，避免了不必要的请求

SPDY只能以IP为单位，进行通信上的多路复用，一个IP多个域名，改善效果就不明显。

### 9.2 全双工通信的WebSocket

Ajax技术： web页面局部更新的异步通信手段，Ajax可以实时地从服务端获取更新内容。

Comet技术： 服务端延迟响应客户端的请求，直到服务端有内容更新为止。

这两种技术并不能从根本上解决HTTP的问题。WebSocket就是为了解决这个问题而诞生的。

一旦客户端和服务端建立WebSocket连接，以后所有的通信都通过WebSocket传输，数据可以是：JSON，XML，HTML、image等任意格式的数据。WebSocket协议有以下特点：

- 推送功能：支持服务端推送数据
- 减少了通信量：一旦建立WebSocket通信，连接就一直保持，通信量减少了

WebSocket通信的建立

为了实现WebSocket的通信，需要使用HTTP协议的Upgrade首部，告知服务端通信协议改变。

```
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-Websocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Origin: http://example.com
Sec-Websocket-Protocol: chat, superchat
Sec-Websocket-Version: 13
```

Sec-Websocket-Key记录着通信键值，Sec-Websocket-Protocol记录了使用的子协议。

服务端返回101 Switching Protocols响应

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-Websocket-Accept: s3PLMBiTxaQ9kYGzzhZRbK+XOo=
Sec-Websocket-Protocol: chat
```

Sec-Websocket-Accept的值是由Sec-Websocket-Key字段值生成的，握手成功之后，使用WebSocket独立的数据帧。

![WebSocket](./images/WebSocket.bmp)

### 9.3 HTTP 2.0

HTTP围绕7项技术进行讨论（AD 2011）

- 压缩
- 多路复用
- 协商
- 客户端拉曳/服务器推送
- 流量控制
- WebSocket

[Back](#目录)

## 第十章 构建Web的技术

### 10.1 HTML

web页面是由HTML编写的，经过浏览器渲染之后，呈现给用户。

CSS样式表，赋予web页面丰富多彩的样式。

### 10.2 动态HTML

动态HTML主要依靠Javascript来改变HTML和CSS。DOM是JavaScript操作HTML的API。

### 10.3 Web应用

Web应用是通过Web提供服务的应用程序，得益于动态HTML的出现，web应用越来越流行。

![动态内容和静态内容](./images/动态内容和静态内容.bmp)

CGI(Common Gateway Interface)：web服务器接收到客户端的请求后，转发给程序的一组机制。

Servlet：一种能在服务端创建动态内容的程序。

### 10.4 数据发布的格式和语言

XML：可以扩展的通用标记语言

JSON：以Javascript中的对象为基础的轻量级数据标记语言，它支持7种类型的数据：false/null/true/object/array/number/string，由于Ajax让json的使用更加广泛。

[Back](#目录)

## 附录

### 1. HTTP首部字段

#### 1.1 通用首部字段

| Name                | Explanation                          |
| :------------------ | :----------------------------------- |
| Cache-Control       | 控制缓存的行为                       |
| Connection          | 连接管理                             |
| Date                | 创建报文的日期时间                   |
| Pragma              | 报文指令                             |
| Trailer             | 报文末端首部                         |
| Transfer-Encoding   | 报文主体编码方式                     |
| Upgrade             | 升级为其它协议                       |
| Via                 | 代理服务器信息                       |
| Warning             | 错误通知                             |

#### 1.2 请求首部字段

| Name                | Explanation                          |
| :------------------ | :----------------------------------- |
| Accept              | 可处理的文件类型                     |
| Accept-Charset      | 优先的字符集                         |
| Accept-Encoding     | 优先的内容编码                       |
| Accept-Language     | 优先的语言                           |
| Authorization       | 认证信息                             |
| Expect              | 期待服务器的特定行为                 |
| From                | 用户的电子邮箱地址                   |
| Host                | 服务端IP地址                         |
| If-Match            | 比较实体标记（ETag）                 |
| If-Modified-Since   | 比较资源更新时间                     |
| If-None-Match       | 比较实体标记，与If-Match相反         |
| If-Range            | 资源未更新时发送的实体Byte的范围请求 |
| If-Unmodified-Since | 比较资源的更新时间                   |
| Max-Forwards        | 最大传输跳帧数                       |
| Proxy-Authorization | 代理服务器要求客户端的认证           |
| Range               | 实体的字节范围请求                   |
| Referer             | 请求中URI的原始获取方                |
| TE                  | 传输码的优先级                       |
| User-Agent          | HTTP客户端程序信息                   |

#### 1.3 响应首部字段

| Name              | Explanation                  |
| ----------------- | ---------------------------- |
| Accept-Ranges     | 是否接收字节范围请求         |
| Age               | 推算资源创建经过时间         |
| ETag              | 资源的匹配信息               |
| Location          | 令客户端重定向至指定URI      |
| Proxy-Authentcate | 代理服务器对客户端的认证信息 |
| Retry-After       | 对再次发起请求的时间要求     |
| Server            | HTTP服务器的安装信息         |
| Vary              | 代理服务器缓存的管理信息     |
| WWW-Authenticate  | 服务器对客户端的认证信息     |

#### 1.4 实体首部字段

| Name             | Explanation            |
| ---------------- | ---------------------- |
| Allow            | 资源可支持的           |
| Content-Encoding | 实体使用的编码方式     |
| Content-Language | 实体的自然语言         |
| Content-Length   | 实体主体的大小（Byte） |
| Content-Location | 替代对应资源的URI      |
| Content-MD5      | 实体主体的报文摘要     |
| Content-Range    | 实体主体的位置范围     |
| Content-Type     | 实体主体的媒体类型     |
| Expires          | 实体主体的过期时间     |
| Last-Modified    | 资源最后的修改时间     |

### 2. OSI 七层网络模型

![osi七层模型](./images/osi七层模型.jpg)

[Back](#目录)