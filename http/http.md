### HTTP报文的组成

### HTTP三次握手

### HTTP四次握手

### HTTP2.0做了哪些优化

1. 多路复用，让多个请求在一个连接上进行
2. 头部压缩，传输头部对应的index，减少了传输的开销
3. 服务端推送，服务端主动推送客户端需要的资源

### HTTPS

### HTTP0.9

需求简单，只用来传输超文本 html，所以请求只有一个请求行。服务器也没有响应头，由于传输的  是 HTML 所以文件以 ASCII 字符流传输。

1. 建立 TCP 连接
2. 发送 GET 请求行信息 `GET /index.html`
3. 服务器收到请求，读取 HTML 文件，以 ASCII 字符流返回
4. 断开 TCP 连接

### HTTP1.0

浏览器要处理的文件不再只是 HTML，还包含了图片、音频、视频、CSS、JavaScript。为了满足多种类型文件的传输，客户端需要和服务器更充分的交流，引入了请求头，响应头。特别时文件类型相关的。

请求头：

```
accept: text/html                   // 文件类型
accept-encoding: gzip,deflate,br    // 压缩方式
accept-charset: ISO_8859-1,utf-8    // 编码方式
accept-language: zh-CN,zh           // 页面优先语言
```

响应头：

```
content-type: text/html; charset=UTF-8
content-encoding: br
```

客户端再请求头中加入期望返回的文件类型，编码方式等。收到响应后再根据响应头信息处理文件。

除了对多类型文件提供良好的支持以外，还新增了以下特性。

1. 响应行中加入了状态码
2. 提供了缓存机制来缓存已经下载过的文件
3. 请求头中加入了用户代理字段

### UDP 和 TCP 的区别

UDP (User Datagram Protocol) 是在基于 IP 能和应用打交道的协议。其中包含了一个最重要的信息**端口号**，UDP正是利用端口号将数据包送达指定的应用。UDP 传输速度快，但没有重发机制，不能保证数据的可靠性。数据包在传输过程中容易丢失，大文件会被拆分成很多小的数据包来传输，这些小的数据包会经过不同的路由，并在不同的时间到达接收端，而 UDP 协议并不知道如何组装这些数据包，从而把这些数据包还原成完整的文件。

TCP 对于数据包丢失的清空，提供了重传机制。并且引入了数据包排序机制用来保证将乱序的数据包组合成一个完整的文件。

### HTTP 缓存

**强缓存**，浏览器会根据 expires(旧的) 和 Cache-control(新的) 头部来判断是否该使用缓存，如果命中缓存，则直接从缓存中获取数据，不再与服务器发生通信。

expires 是一个绝对时间戳，`expires: Wed, 11 Sep 2019 16:12:18 GMT`，服务器响应资源的请求时，会在 Response Header 中写入 `expires`。当浏览器再次向服务器请求资源时，会对比本地时间和 expires 时间戳，如果本地时间小于 expires，则直接从缓存中获取数据，反之则向服务器请求资源。



Cache-Control 有多个选项

1. `max-age=3600` 一个相对时间
2. `private` 资源只能被浏览器缓存
3. `public` 资源可以被浏览器和代理服务器缓存
4. `no-store` 不使用任何缓存，直接向服务器请求资源
5. `no-cache` 不再向浏览器询问缓存情况，而是向服务端确认资源是否过期（<u>协商缓存路线</u>）



**协商缓存**，浏览器会根据 Last-Modified 和 Etag 向服务器询问资源是否过期，从而决定是使用缓存还是重新下载资源。

Last-Modified 是一个绝对时间戳，`Last-Modified: Fri, 27 Oct 2017 06:35:57 GMT`，服务器响应资源的请求时，会在 Response Header 中写入 `Last-Modified`。后面每次请求的请求头中会加上 `If-Modified-Since` 字段，也就是上一次服务器响应时 `Last-Modified`的值。服务器会将资源的最后修改时间和 `If-Modified-Since` 做对比，如果一致说明资源未改动，返回`304`状态码，告诉浏览器使用浏览器缓存。如果不一致说明资源发生改动，则返回新的资源和新的 `Last-Modified`。

Etag 是基于文件内容编码生成的标识的字符串，`ETag: W/"2a3b-1602480f459"`。服务器响应资源的请求时，会在 Response Header 中写入 `Etag`。后面每次请求的请求头中会加上 `If-None-Match` 字段，也就是上一次服务器响应时 `Etag`的值。服务器会将当前文件 `Etag` 和 `If-None_Match`做对比来确认资源是否发生了改动。

### 同源策略

同源策略会限制两个不同源页面之间的 DOM 操作，数据获取（cookie, localStorage, indexDB）和网络请求（XMLHttpRequest）

[同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)

### 跨域

1. CORS 跨域
2. 代理（webpack-dev-server）
3. JSONP
4. Websocket
5. window.postMessage

### CORS 跨域

#### 响应头设置

1. `Access-Control-Allow-Origin`
2. `Access-Control-Allow-Credentials`
3. `Access-Control-Allow-Headers`

#### 简单请求和非简单请求

同时满足以下要求则为简单请求，否侧为非简单请求

1. 请求方法为下列方法之一
   - GET
   - HEAD
   - POST
2. 头部信息不能是一下之外的
   - Accept
   - Accept-Language
   - Content-Language
   - Content-Type
3. Content-Type 为一下三种之一
   - text/plan
   - multipart/form-data
   - application/x-www-form-urlencoded

如果是非简单请求，浏览器会先发出一个预检(preflight)请求，预检请求的方式为 OPTIONS

[mdn](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS)

[软一峰 CORS](http://www.ruanyifeng.com/blog/2016/04/cors.html)