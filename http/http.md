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