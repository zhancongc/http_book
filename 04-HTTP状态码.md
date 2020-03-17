## 第四章 HTTP状态码

[TOC]

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