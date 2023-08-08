# HTTP 协议

# 1. HTTP

## 1.1 HTTP 的概念

**HTTP 的概述：**

-   HTTP：超文本传输协议（HyperText Transfer Protocol）。
-   HTTP 基于 TCP 协议，默认占用 80 端口。
-   HTTP 支持传输多种格式：文本、图片、视频等。
-   HTTP 使用明文传输，安全性不高。

**HTTP 的传输过程：**

1.  客户端与服务器建立 socket 连接。
2.  客户端发送文本形式的请求报文。
3.  服务器处理请求，发送响应报文。
4.  释放 TCP 连接。

**HTTP 报文结构：**

-   首行 + 头部（Header）+ 正文（Body）
-   头部和正文之间，有一个空行作为分隔。

### 1.1.1 HTTP 的特点

**无连接：**

-   无连接：每次连接只能处理一个请求，处理完请求，就断开连接。
-   HTTP 1.1 版本，默认为持久连接。一次连接可以发送多个请求。

**无状态：**

-   每次请求都是独立的，多个请求之间不会共享数据。
-   使用 Cookie 可以保持状态，在请求头携带 Cookie，就能实现登录保持。

## 1.2 请求

### 1.2.1 请求报文格式

第一行为请求行，在请求头和请求体之间，有一个空行。

格式：

```http
请求方法 URL 协议版本
请求头

请求体（发送数据）
```

示例：

```http
POST /login HTTP/1.1
Content-Type: applicationi/x-www-form-urlencoded
Content-Length: 18

name=admin&pwd=123
```

### 1.2.2 请求方法

HTTP/1.1 定义了 8 种方法，以不同的方式操作指定的资源。

注意：方法名称区分大小写。

| **请求方法**    | **作用**                |
| ----------- | --------------------- |
| **GET**     | 请求资源。                 |
| **POST**    | 提交数据。                 |
| **HEAD**    | 只获取响应头。               |
| **PUT**     | 更新指定资源。               |
| **DELETE**  | 删除指定资源。               |
| **PATCH**   | 对 PUT 的补充，对已有资源局部更新   |
| **TRACE**   | 回显服务器收到的请求，主要用于测试和诊断。 |
| **OPTIONS** | 列出资源支持的请求方法。          |
| **CONNECT** | 建立连接隧道，与代理服务器进行通信。    |

#### 1.2.2.1 GET 和 POST

**GET 和 POST 的区别：**

-   GET 用于获取数据，刷新无害；POST 用于提交数据，刷新将重新提交。
-   GET 的数据在 URL 中，是可见的，不够安全；POST 放在请求体中，不可见。
-   GET 提交的数据有大小限制（URL 长度有限），POST 则没有限制。
-   GET 只能接受 ASCII 字符，需要进行 URL编码，POST 没有限制。

**关于 URL 长度限制：**

-   HTTP 协议本身没有长度限制，是浏览器和服务器自身进行的限制。

**POST 发送两个数据包：**

-   POST 请求会将 Header 和 Body 分开发送，先发送 Header，服务器返回 100 后，再发送 Body。
-   HTTP 协议没有规定 POST 会产生两个 TCP 数据包，这是部分浏览器和框架的请求方法。

### 1.2.3 请求头字段

字段名规则：

-   不区分大小写。
-   字段名不允许出现空格和下划线，使用 `-` 作为单词分隔。
-   字段名后面紧跟 `:`，没有空格，`:` 后可以有空格。

常见字段：

-   User-Agent：客户端软件名和版本等信息。
-   Cookie：发送请求时，携带该域名下的 cookie。
-   Referer：上一个页面的 URL。
-   Accept：客户端支持的数据类型。
-   Accept-Charset：浏览器支持的字符集。
-   Accept-Encodign：客户端支持的编码。
-   Accept-Language：客户端支持的语言。
-   Cache-Control：指定请求和相应遵循的缓存机制。如：`Cache-Control：no-cache`
-   Content-Length：请求内容的长度。
-   Content-Type：请求实体对应的 MIME 信息。
-   Host：目标服务器的域名和端口号。

## 1.3 响应

### 1.3.1 响应报文格式

响应报文的第一行是响应行（状态行）。

格式：

```http
协议版本 状态码 状态短语
响应头

响应体
```

示例：

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1024

<html>
...
```

### 1.3.2  状态码

状态码使用三位数字表示，第一位数字表示类型。

**状态码的分类：**

| **分类**  | **含义**                   |
| ------- | ------------------------ |
| **1xx** | 消息，服务器收到请求，需要请求者继续执行操作。  |
| **2xx** | 成功，请求被服务器接收并处理。          |
| **3xx** | 重定向，需要进一步的操作以完成请求。       |
| **4xx** | 客户端错误，请求包含语法错误或者无法被执行。   |
| **5xx** | 服务器内部错误，服务器在处理正确请求时发生错误。 |

**常见状态码：**

-   2xx：
    -   200：请求成功。【OK】
    -   201：成功创建新的资源。【Created】
    -   202：成功接收请求，但未处理完成。【Accepted】
-   3xx：
    > 301 和 302 通过 Location 字段声明要跳转的 URL。
    -   301：永久重定向，请求的资源不存在，使用新的 URL 访问。【Moved Permanently】
    -   302：临时重定向，请求的资源还在，但是需要用另一个 URL 访问。【Found】
-   4xx：
    -   400：请求报文有错误。【Bad Request】
    -   401：请求需要身份认证。【Unauthorized】
    -   403：服务器拒绝请求。【Forbidden】
    -   404：请求的资源不存在。【Not Found】
    -   405：请求的方法不被允许。【Method Not Allowed】
    -   406：请求的条件无法满足。【Not Acceptable】
    -   408：请求超时。【Request Time-out】
-   5xx：
    -   500：服务器内部错误。【Internal Server Error】
    -   503：服务器忙碌，无法响应服务。【Service Unavailable】

### 1.3.3 响应头字段

-   Allow：服务器支持的请求方法。
-   Content-Encoding：文档的编码方法。
-   Content-Length：内容长度。
-   Content-Type：响应内容的类型。
-   Set-Cookie：设置 Cookie。
-   Location：请求资源的绝对路径。



# 2. URL

URL：统一资源定位符，唯一标识一个资源（文字、视频、音频等）

**URL 的格式：**

**protocol:****//****user:passwd@****host\[:port]****/path****\[?query]****#fragment**

-   **protocol**：传输协议，如：http。
-   **user:passwd：**用户名、口令。
-   **host**：主机地址，域名或IP地址
-   **port**：端口号，http协议默认80端口
-   **path**：路径，网络资源在服务器中的路径
-   **query**：查询字符串，GET 方法的请求参数。
-   **fragment**：片段（锚点），页面滚动到锚点位置。

## 2.1 查询字符串

**格式：**

-   以 `?` 分割路径和数据，参数之间用 `&` 相连，用 `=` 分隔参数名和数据。
-   如：`xx.com?name=admin&age=18`

**URL 编码：**

-   URL 只能使用 ASCII 编码，所有非 ASCII 码字符转为十六进制数值，在每个十六进制数值前面加 `%`。
-   如：`keyword=%E5%90%83%E7%93%9C`



# 3. **Cookie**

**Cookie 的作用：**

-   Cookie可以解决 HTTP 无状态的问题。
-   Cookie 可以用于身份识别和广告追踪。

**关于 Cookie：**

-   Cookie 是存储在浏览器中，使用键值的文本形式存储。
-   同一个域名下发送的请求，都会携带同样的 Cookie。
-   客户端接收到响应头的 `Set-Cookie` 字段，将会写入 Cookie。

请求头：

```http
Cookie: a=xx;b=xx

```

响应头：

```http
Set-Cookie: a=xx
Set-Cookie: b=xx
```

**Cookie 的生命周期：**

-   通过 Expires 和 Max-Age 两个属性来设置。
-   Expires：过期时间，Max-Age：持续时间，单位：秒。
-   Cookie 过期后，将被浏览器删除。

**Cookie 的作用域：**

-   通过 Domain 和 path 属性设置。
-   只有域名和路径两个属性匹配，但会携带 Cookie。
-   `path=/` 表示域名下的任意路径都携带 Cookie。

**Cookie 的缺点：**

-   容量缺陷，只有 4 kb。
-   性能缺陷，每次请求都要携带 Cookie。
-   安全缺陷：Cookie 以纯文本的形式传输，容易被截获。



# 4. 跨域问题

**同源政策：**

-   浏览器与服务器的请求遵循同源政策。
-   同源：相同的协议、主机、端口。

**非同源的限制：**

-   不能读取和修改对方的 DOM。
-   不能访问对方的 Cookie、IndexDB、LocalStorage。
-   限制 XMLHttpRequest 请求。

**跨域：**

-   当浏览器对非同源 URL 发送 Ajax 请求时，就会产生跨域，称为跨域请求。

#### 1.1.3.1 CORS

**关于 CORS：**

-   CORS（跨域资源共享）是一个 W3C 的标准。
-   CORS 只需浏览器和服务器共同支持。
-   CORS 分为简单请求和非简单请求。

**简单请求：**

-   请求方法为 GET、PSOT、HEAD。
-   请求头中，添加 Origin 字段，说明请求来自哪个源。
-   响应头中，添加 Access-Control-Allow-Origin 字段，如果 Origin 不在这个字段范围内，浏览器将响应拦截。
-   Access-Control-Allow-Credentials：是否允许发送 Cookie，默认为 false。
