### XSS攻击

XSS是跨站脚本攻击，恶意攻击者往web页面插入script代码，当用户浏览该页时就会执行，达到恶意攻击用户的目的。

XSS：跨站脚本攻击。就是攻击者想尽办法把执行代码注入到网页中。

* 存储型：常见带有用户保存数据的网站，如论坛评论，商品评论，用户私信等。
  + 攻击者将代码提交到目标用户的数据库
  + 用户打开目标网站，网站服务端将恶意代码从数据库取出，拼接在HTML返回给浏览器
  + 用户浏览器接收到响应后立即执行，混在其中的恶意代码也被执行

* 反射型：将恶意代码在URL上。
  + 攻击者构造出特殊的url，其中包含恶意代码
  + 用户打开带有恶意代码的url，网站服务端将恶意代码从url取出，拼接在Html中返给浏览器
  + 用户浏览器接收到响应后解析执行，混在其中的恶意代码会被执行
  + 恶意代码窃取用户数据并发送到窃取者网站，或者冒充用户行为，调用目标网站接口执行攻击者指定操作。

* DOM型：取出和执行恶意代码由浏览器端完成，属于JS自身漏洞。 
  + DOM型攻击中，取出和执行恶意代码由浏览器完成。属于前端JS自身的安全漏洞，其它两种XSS都属于服务端的安全漏洞。

解决：XSS攻击两大要素

* 攻击者提交恶意代码
* 浏览器执行恶意代码

针对第一个要素：可以在用户输入的过程，过滤掉用户输入的恶意代码。

预防：

* 对于不受信的输入，限制一个合理的长度。进行校验。
* HTTP-only Cookie禁止SJ读取某个敏感Cookie
* 验证码：防止脚本冒充用户提交为危险操作

XSS检测：

* 使用XSS攻击字符串手动检测XSS漏洞
* 使用扫描工具自动检测XSS漏洞

Content-Security-Policy：

CSP的实质就是白名单制度，开发者明确告诉客户端，哪些外部资源可以加载和执行，等同白名单。它的实现由浏览器完成，开发者只需要提供配置。

两种方法可以启用CSP

* Http头信息 `Content-Security-Policy`
```
Content-Security-Policy: script-src 'self'; object-src 'none';
style-src cdn.example.org third-party.org; child-src https:
```
* 通过meta标签

启用后，不符合CSP的外部资源就会被阻止加载。

限制选项：

* script-src：外部脚本
* style-src样式表
* img-src图像
* media-src媒体文件
* font-src字体文件
* object-src插件
* child-src框架
* frane-ancestors嵌入外部资源
* connect-src  HTTP连接（通过XHR、WebSockets、EventSource等）
* worker-src worker脚本
* manifest-src  mainfest脚本


XSS总结：

* 利用模板引擎：开启模板引擎自带的html转义功能
* 避免内联事件
* 避免拼接HTML
* 增加攻击难度，降低攻击后果：通过GSP、输入长度限制、接口安全措施等
* 主动检测和发现
