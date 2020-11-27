* http请求头

  + Accept
    - Accept:text/html   // 浏览器可以接收服务器回发
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

  + Content-Disposition:响应头指示回复的内容以何种形式展示，是以内联的形式（网页或者网页的一部分），还是以附件的形式下载到本地
    - Content-Disposition:inline
    - Content-Disposition:attachment
    - Content-Disposition:attachment;filename = 'filename.jpg'
   

  + Content-Encoding：
  + Date: 服务端发送资源时的服务器时间
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


#### 文件上传下载相关

