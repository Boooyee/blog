### get个post区别：

* 语义不同，get表示读取资源，post是提交资源
* 在浏览器中get反复读取不应该对访问的数据有副作用（幂等的），因为是读取，可以对GET请求进行缓存。缓存可以是在浏览器上，也可以是在代理上或者server端。post是不幂等的，有副作用。不能随意多次执行，不能缓存。

* 携带数据格式有区别，GET在url query携带参数，POST在body携带数据。浏览器对query携带数据有限制。

* 浏览器的post会发两个请求吗？
  + 使用Http有个约定，‘控制类’信息放在请求头中，具体的数据放在请求体。客户端做了一些优化，比如数据超过一定值就先发请求头，否则一次性全发，因此到底是发一次还是两次是由客户端决定的，不管怎么发都符合Http协议。

### Http状态码

* 1XX：表示请求已被接受，但需要后续处理
  + 100：客户端应继续发送请求
  + 101：需要切换协议，服务器通过Upgrade响应头字段通知客户端。HTML5引入的websocket就是这样工作的，客户端首先请求websocket所在的Url，服务器返或101. 然后便建立起来全双工的TCP连接。注意Upgrade和Connection字段属于Hop-by-hop字段，设置websocket代理需要继续设置这两个字段，而不是简单的转发请求。

* 2XX：请求已成功被服务器接收，理解，并接受
  + 200：请求成功
* 3XX：客户端采取进一步的操作才能完成请求，通常，这些状态码用来重定向，重定向目标在本次响应的Location字段指明。
  + 301：请求资源已被永久转移到新位置。301通常用于网站迁移时，服务器对旧的URL进行301重定向到新的URL。
  + 302：临时性转移
  + 304：未修改（Not Modified）
* 4XX：客户端错误
  + 400：语法错误，请求无法被服务器理解
  + 401：当前请求需要用户验证，响应体需要包含一个 `www-Authenticate` 字段来询问用户的授权信息。客户端的下次请求需要提供包含Authorization头的请求。
  + 403：服务器理解，拒绝执行。属于前端bug
  + 404：资源不存在
* 5XX：服务器处理请求过程中有错误或者异常发生
  + 500后台bug
  + 502作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应
  + 504（Gateway Time-out）作为网关或者代理服务器尝试执行请求时，未能从上游服务器收到响应

### 浏览器缓存

* 强缓存
  + Expires：表示缓存到期时间，是个绝对时间。如果修改了客户端的本地时间，会导致缓存失效
  + cache-control：http1.1新增的相对时间。

    - max-age：相对时间
    - no-cache：需要进行协商缓存，发送请求到服务器确认是否使用缓存
    - no-store：禁止使用缓存，每一次都要重新发送请求
    - public：默认设置
    - private：不能被多用户共享

* 协商缓存

  + Last-Modified、If-Modified-Since：第一次请求时，服务器返回header上带有last-Modified字段，表示资源的最后修改时间。再次请求资源时请求头带有If-Modified-Since字段。服务器收到请求，进行比较。如果相等，表明资源未修改，返回304，浏览器使用本地缓存。
    - 最小时间是秒，如果在秒内资源发生修改，Last-Modified不会发生变化
    - 周期变化，如果资源在一个周期内改回原来的样子，我们认为可以使用缓存，但是Last-Modified不是这样认为。 
  + Etag、If-None=Match：浏览器第一次发送请求获得etag值，下一次浏览器再请求request header中带上if-none-match（etag值），服务器比较etag值，判断是否使用缓存。etag每次服务器生成都需要进行读写操作，消耗更大。

* 开启keep-Alive：能够减少浏览器与服务器建立连接的次数，节省建立连接的时间。
  + keep-alive模式（持久连接，连接重用），keep-alive功能使得客户端到服务端连接持续有效，后续请求避免了重新建立连接。
  + http 1.0默认是关闭的，如果客户端浏览器支持keep-alive，在http请求头添加一个字段Connection:keep-alive字段，当服务器收到附带keep-alive的请求时，它也会在响应头中添加一个同样的字段来使用keep-alive。这样一来，客户端和服务端HTTP连接就会保持，不会断开（超过keep-alive时间除外），当客户端发起另一个请求时，就使用这条已经建立起来的连接。
  + 在http1.1中默认启用keep-alive，除非在请求头或者响应头指明要关闭Connection:close。

* http请求头
  + Accept
    - Accept:test/html
    - Accept:*/*
  + Accept-Encoding
    - Accept-Encoding：gzip,deflate  浏览器申明接收的编码方式，通常指定压缩方法，是否支持压缩，支持什么压缩。
  + Accept Language
    - 浏览器申明接收的语言
  + Connection：
    - Connection：keep-alive
    - Connection：close 代表一个Request完成后，客户端和服务器之间传输HTTP数据的TCP连接关闭。当客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。
  + Host：
    - Host：www.baidu.com 请求报头域主要用于指定被请求资源的Internet主机和端口号，通常从HTTP URL中提取出来。
  + Referer：
    - 当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器是从哪个页面链接过来的
  + User-Agent：告诉HTTP服务器，客户端使用的操作系统和浏览器的名称和版本
  + Cache-Control：
    - Cache-Control:private  只能作为私有的缓存，不能在用户间共享
    - Cache-Control:public 响应会被缓存，并且在用户间共享。
    - Cache-Control:no-cache  响应不会被缓存，而是实时向服务器请求资源
    - Cache-Control:no-store  在任何条件下，响应都不会被缓存
  + Cookie：用来存储用户信息，让服务器辨别用户身份。
  + Range：断点续传
    - Range:bytes=0-5 指定第一个字节和最后一个字节的位置，用于高速服务器自己想获取对象的哪部分。

* http响应头
  + Cache-Control：
  + Content-Type：
    - Content-Type: text/html;chartset=UTF-8  告诉客户端，资源的文件类型，字符编码，客户端通过utf-8对资源进行解码，然后对资源进行html解析。
  + Content-Encoding：
  + Date:服务端发送资源时的服务器时间
  + Server：
    - Server:Tengine/1.4.6  服务器和相对应的版本，只是告诉客户端服务器信息。
  + Expires：
  + Last-Modified
  + Etag

* Content-Security-Policy（XSS攻击）
* upgrade-insecure-requests
* Origin Header（CSRF攻击）
* Referer Header（CSRF攻击）

* X-Content-Type-Options（防止上传HTML内容被解析为网页）



### https/http2

* http存在的问题
  - 内容使用明文（不加密），内容可能被窃听
  - 无法证明报文的完整性，所以可能遭到篡改
  - 不能验证通信方的身份，可能遭遇伪装
  - 高延迟-页面加载速度降低
  - 无状态-巨大的http头部
  - 队头阻塞问题（HTTP层和TCP层都会发生）

* https http+SSL/TLS层

* HTTP2
  - 二进制传输：HTTP2传输数据量的大幅度减少，主要有两个原因：二进制传输和Header压缩。
  - Header压缩
  - 多路复用：
    - HTTP1.x中，如果客户端发送多个并行的请求，必须使用多个TCP连接。HTTP2.0实现了多向请求和响应：客户端和服务端可以把HTTP消息分解为互不相认的帧，然后乱序发送。
  - Server Push：服务器推送
  - 提高安全性
  - 解决了HTTP层队头阻塞问题
* HTTP3新特性
  - HTTP跑在QUIC上而不是TCP上
  - QUIC新功能
    - 实现了类似TCP流量控制，传输可靠性功能
    - 实现了快速握手
    - 集成了TLS加密
    - 多路复用：彻底解决了TCP中队头阻塞问题


* 对称加密：通信双方持有同一个密钥，加密和解密都是使用这一个密钥进行。
  - 优点：加密解密速度快，不会造成性能上太大的损失。
  - 缺点：商定加密规则的过程中是不安全的。

* 非对称加密：非对称加密存在两个密钥，一个私钥一个公钥。既可以用公钥加密，也可以用私钥加密。
  - 公钥是任何人都能获取的，只有私钥能解密。
  - 弊端：必须保证私钥不能泄露。非对称加密性能损耗大，存在中间人攻击。

* https握手过程
  - client-hello阶段，浏览器解析域名获取IP默认与此Host 443端口进行连接。将支持的加密组件、Host头信息发送给服务器。附带随机数生成的session tick1
  - server-hello阶段，根据发送的host寻找服务器证书，然后将服务器证书、加密方案、随机的session ticket发送给浏览器
  - cipher-spec阶段，浏览器接到证书后验证有效性，若通过检查生成一份session ticket(3),通过返回的公钥，用协商的加密算法加密。返回给服务器。同时浏览器用session ticket(1) session ticket(2) session ticket(3)合成session key.

  服务器接到后，用私钥解密session ticket(3),同样合成session key。
  - 内容传输阶段：至此TLS连接建立，连接销毁前，浏览器和服务器通过session key进行对称加密。
  