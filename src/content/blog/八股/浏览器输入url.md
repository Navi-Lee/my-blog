---
title: "浏览器输入url会发生什么？"
description: ""
pubDate: "September 16 2026"
tags: ["JS", "计算机网络"]
Class: ["前端算法手写八股", "技术相关"]
---

# 浏览器输入 URL 后发生了什么

假设浏览器访问：

```text
https://www.example.com
```

整体流程可以先记成：

```text
输入 URL
↓
解析 URL
↓
DNS 解析域名
↓
得到服务器 IP
↓
建立 TCP 连接
↓
进行 TLS 握手
↓
发送 HTTP 请求
↓
服务器处理请求
↓
返回 HTTP 响应
↓
浏览器解析资源
↓
渲染页面
```

---

## 1. 解析 URL

**URL：Uniform Resource Locator**

中文：

> 统一资源定位符

例如：

```text
https://www.example.com/index.html
```

可以拆成：

```text
https             → 协议
www.example.com   → 域名
/index.html       → 路径
```

浏览器首先需要知道：

> 使用什么协议，访问哪个服务器上的哪个资源。

---

## 2. DNS 解析域名

浏览器现在知道：

```text
www.example.com
```

但网络通信最终需要目标服务器的 IP 地址。

因此需要进行 DNS 解析。

**DNS：Domain Name System**

中文：

> 域名系统

DNS 的核心作用：

```text
域名
↓
IP 地址
```

例如：

```text
www.example.com
↓
93.xxx.xxx.xxx
```

可以类比：

```text
域名 = 联系人姓名
IP   = 电话号码
DNS  = 通讯录
```

---

## 3. 根据 IP 找到目标服务器

**IP：Internet Protocol**

中文：

> 网际协议

拿到服务器 IP 地址以后，数据可以通过互联网中的路由设备逐步发送到目标服务器。

简化理解：

```text
你的电脑
↓
本地路由器
↓
运营商网络
↓
中间路由器
↓
目标服务器
```

IP 主要负责：

> 确定数据应该发送到哪台机器。

---

## 4. 建立 TCP 连接

**TCP：Transmission Control Protocol**

中文：

> 传输控制协议

HTTPS 通常基于 TCP。

TCP 是面向连接的，所以正式传输 HTTP 数据之前，需要先建立 TCP 连接。

也就是之后要学习的：

> TCP 三次握手

可以先简单理解：

```text
客户端：我要建立连接
服务器：可以
客户端：收到，开始通信
```

TCP 连接建立之后：

```text
客户端
⇄
服务器
```

就可以进行可靠的数据传输。

---

## 5. 进行 TLS 握手

如果访问的是：

```text
https://
```

还需要建立安全的加密通信。

**TLS：Transport Layer Security**

中文：

> 传输层安全协议

TLS 主要负责：

- 验证服务器身份
- 协商加密方式
- 生成通信所需的密钥
- 为后续通信提供加密保护

可以理解为：

```text
TCP
负责建立可靠连接

TLS
负责在 TCP 连接之上建立安全通道
```

因此 HTTPS 常见的关系是：

```text
HTTP
↓
TLS
↓
TCP
↓
IP
```

可以简单记：

> HTTPS = HTTP + TLS

---

## 6. 浏览器发送 HTTP 请求

**HTTP：HyperText Transfer Protocol**

中文：

> 超文本传输协议

连接建立以后，浏览器开始向服务器发送 HTTP 请求。

例如：

```http
GET / HTTP/1.1
Host: www.example.com
```

这个过程叫：

**HTTP Request**

中文：

> HTTP 请求

意思大致是：

> 请求获取这个网站的首页资源。

---

## 7. 服务器处理请求

服务器收到 HTTP 请求后，会根据请求内容进行处理。

例如可能进行：

```text
读取服务器代码
↓
查询数据库
↓
处理业务逻辑
↓
生成返回结果
```

这些主要属于后端处理逻辑。

---

## 8. 服务器返回 HTTP 响应

服务器处理完请求后，会向浏览器返回 HTTP 响应。

这个过程叫：

**HTTP Response**

中文：

> HTTP 响应

例如：

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

其中：

```text
200
```

属于 HTTP 状态码。

后面可以单独学习：

- 200
- 301
- 302
- 304
- 400
- 401
- 403
- 404
- 500

---

## 9. 浏览器继续请求其他资源

浏览器拿到 HTML 后，会解析 HTML。

例如：

```html
<html>
  <head>
    <link rel="stylesheet" href="style.css" />
  </head>

  <body>
    <script src="app.js"></script>
  </body>
</html>
```

浏览器发现 HTML 中还引用了：

```text
CSS
JavaScript
图片
字体
其他资源
```

于是会继续发送网络请求获取这些资源。

因此一个网页通常不只有一个 HTTP 请求。

可能会包含：

```text
HTML 请求
CSS 请求
JS 请求
图片请求
字体请求
接口请求
……
```

---

## 10. 浏览器渲染页面

当浏览器拿到：

```text
HTML
CSS
JavaScript
```

之后，会开始解析并渲染页面。

这里就逐渐进入浏览器原理，例如：

```text
DOM
CSSOM
Render Tree
Layout
Paint
Composite
```

这些主要属于浏览器渲染相关知识，不属于纯计算机网络内容。

---

# 最核心的一条主线

现在阶段重点记：

```text
DNS
↓
TCP
↓
TLS
↓
HTTP
```

分别代表：

```text
DNS
域名 → IP

TCP
建立可靠连接

TLS
建立安全加密通信

HTTP
客户端与服务器进行请求和响应
```

---

# 简化版完整流程

```text
1. 浏览器解析 URL

2. DNS 将域名解析成 IP 地址

3. 根据 IP 找到目标服务器

4. 建立 TCP 连接

5. 如果是 HTTPS，进行 TLS 握手

6. 浏览器发送 HTTP Request

7. 服务器处理请求

8. 服务器返回 HTTP Response

9. 浏览器获取 HTML、CSS、JavaScript 等资源

10. 浏览器解析并渲染页面
```

---

# 面试简答版

> 浏览器输入 URL 后，首先会解析 URL，然后通过 DNS 将域名解析为 IP 地址。接着与目标服务器建立 TCP 连接。如果使用 HTTPS，还需要进行 TLS 握手建立加密通信。连接建立后，浏览器发送 HTTP 请求，服务器处理请求并返回 HTTP 响应。浏览器收到 HTML、CSS、JavaScript 等资源后，最终解析并渲染页面。

---

# 本节英文缩写

| 缩写  | 全称                               | 中文                 |
| ----- | ---------------------------------- | -------------------- |
| URL   | Uniform Resource Locator           | 统一资源定位符       |
| DNS   | Domain Name System                 | 域名系统             |
| IP    | Internet Protocol                  | 网际协议             |
| TCP   | Transmission Control Protocol      | 传输控制协议         |
| TLS   | Transport Layer Security           | 传输层安全协议       |
| HTTP  | HyperText Transfer Protocol        | 超文本传输协议       |
| HTTPS | HyperText Transfer Protocol Secure | 安全的超文本传输协议 |
| HTML  | HyperText Markup Language          | 超文本标记语言       |
| CSS   | Cascading Style Sheets             | 层叠样式表           |
| DOM   | Document Object Model              | 文档对象模型         |
| CSSOM | CSS Object Model                   | CSS 对象模型         |
