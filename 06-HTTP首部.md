## 第六章 HTTP首部

[TOC]

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