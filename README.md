# 《图解HTTP协议》摘要（春水版）

## [第一章 web及网络基础](https://github.com/zhancongc/http/blob/master/01-web%E5%8F%8A%E7%BD%91%E7%BB%9C%E5%9F%BA%E7%A1%80.md)
### 1.1 使用http访问web
### 1.2 http诞生
### 1.3 TCP/IP协议
##### 1.3.1 TCP/IP协议分层
##### 1.3.2 TCP/IP通信过程
### 1.4 HTTP相关的协议：IP、TCP、DNS
##### 1.4.1 IP协议
##### 1.4.2 可靠的TCP协议
##### 1.4.3 DNS服务
### 1.5 URI

## [第二章 简单的HTTP协议](https://github.com/zhancongc/http/blob/master/02-%E7%AE%80%E5%8D%95%E7%9A%84HTTP%E5%8D%8F%E8%AE%AE.md)
### 2.1 客户端和服务端之间的通信
### 2.2 通过请求和响应的交换达成通信
### 2.3 HTTP是不保存状态的协议
### 2.4 请求URI定位资源
### 2.5 与服务端交互的方法
### 2.6 HTTP/1.1支持的方法
### 2.7 持久连接节省通信量
### 2.8 cookie

## [第三章 HTTP报文内的信息](https://github.com/zhancongc/http/blob/master/03-HTTP%E6%8A%A5%E6%96%87%E5%86%85%E7%9A%84%E4%BF%A1%E6%81%AF.md)
### 3.1 HTTP报文
### 3.2 编码提升传输速率
##### 3.2.1 报文实体
##### 3.2.2 内容编码
##### 3.2.3 分块传输编码
### 3.3 发送多种数据的多部分对象集合
### 3.4 范围请求
### 3.5 返回最合适的内容

## [第四章 HTTP状态码](https://github.com/zhancongc/http/blob/master/04-HTTP%E7%8A%B6%E6%80%81%E7%A0%81.md)
### 4.1 状态码
### 4.2 Success
### 4.3 Redirection
### 4.4 Client Error
### 4.5 Server Error

## [第五章 与HTTP相关的Web Server](https://github.com/zhancongc/http/blob/master/05-%E4%B8%8EHTTP%E7%9B%B8%E5%85%B3%E7%9A%84Web%20Server.md)
### 5.1 Virtual Host
### 5.2 通信数据转发
##### 5.2.1 代理
##### 5.2.2 网关
##### 5.2.3 隧道
### 5.3 保存资源的缓存

## [第六章 HTTP首部](https://github.com/zhancongc/http/blob/master/06-HTTP%E9%A6%96%E9%83%A8.md)
### 6.1 报文首部
### 6.2 HTTP首部字段

## [第七章 更安全的HTTPS](https://github.com/zhancongc/http/blob/master/07-%E6%9B%B4%E5%AE%89%E5%85%A8%E7%9A%84HTTPS.md)
### 7.1 HTTP的缺点
##### 7.1.1 窃听
##### 7.1.2 伪装
##### 7.1.3 篡改
### 7.2 HTTPS
##### 7.2.1 HTTPS是身披SSL外壳的HTTP
##### 7.2.2 加密方式
##### 7.2.3 HTTP使用混合加密方式
##### 7.2.4 如何证明公钥的正确
##### 7.2.5 SSL证书
### 7.3 HTTPS通信
##### 7.3.1 通信过程
##### 7.3.2 SSL速度慢

## [第八章 用户访问认证](https://github.com/zhancongc/http/blob/master/08-%E7%94%A8%E6%88%B7%E8%AE%BF%E9%97%AE%E8%AE%A4%E8%AF%81.md)
### 8.1 何为认证
### 8.2 BASIC认证
### 8.3 DIGEST认证
### 8.4 SSL客户端认证
### 8.5 表单认证

## [第九章 基于HTTP功能的追加协议](https://github.com/zhancongc/http/blob/master/09-%E5%9F%BA%E4%BA%8EHTTP%E5%8A%9F%E8%83%BD%E7%9A%84%E8%BF%BD%E5%8A%A0%E5%8D%8F%E8%AE%AE.md)
### 9.1消除HTTP瓶颈的spdy
### 9.2 全双工通信的WebSocket
### 9.3 HTTP 2.0

## [第十章 构建Web的技术](https://github.com/zhancongc/http/blob/master/10-%E6%9E%84%E5%BB%BAWeb%E7%9A%84%E6%8A%80%E6%9C%AF.md)
### 10.1 HTML
### 10.2 动态HTML
### 10.3 Web应用
### 10.4 数据发布的格式和语言

## [附录](https://github.com/zhancongc/http/blob/master/11-%E9%99%84%E5%BD%95.md)
### 1. HTTP首部字段
##### 1.1 通用首部字段
##### 1.2 请求首部字段
##### 1.3 响应首部字段
##### 1.4 实体首部字段
### 2. OSI 七层网络模型
