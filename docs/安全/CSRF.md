### CSRF

CSRF：跨站请求伪造，攻击者诱导受害者进入第三方网站，在第三方网站中，向被攻击网站发送跨站请求。

典型的CSRF攻击流程：

* 受害者登陆了a.com，并保留了登录凭证
* 攻击者引诱受害者访问了b.com
* b.com向a.com发送了一个请求，浏览器会默认携带a.com的Cookie
* 服务端接收到请求后，误以为是a.com发送的请求
* 攻击完成后，攻击者在受害者不知情的情况下冒充受害者

集中常见的攻击类型：

* GET类型的CSRF
* POST类型的CSRF：这种类型攻击通常是使用自动提交的表单
* 链接类型CSRF：通常是在图片等嵌入恶意链接，引诱用户点击

CSRF的特点：

* 攻击一般发起在第三方网站，而不是被攻击网站
* 攻击利用受害者在被攻击网站登录凭证，冒充受害者提交操作，而不是直接窃取数据
* 整个过程中并不能获取受害者登录凭证，只是冒用

#### 防护策略：

CSRF两个特点：

* CSRF发生在第三方域名
* CSRF不能获取到Cookie只能使用

针对这两点，可以专门定制防护策略

* 阻止不明外域访问
  + 同源检测
  + Samesite Cookie
* 提交时要求附加本域才能获取信息：
  + CSRF Token
  + 双重Cookie验证

同源策略：

CSRF大多数来自第三方网站，可以直接禁止外域发起请求。

在HTTP协议中，每一个异步请求都携带两个header，用于标记来源域名。

* Origin Header
  + 如果Origin存在，可以直接使用Origin字段确认来源域名
  + 在IE11不会在CORS请求上添加Origin头，Referer头是唯一标识。
  + 302重定向之后Origin不包含在重定向请求中
* Referer Header

#### CSRF Token：

* CSRF Token输入到页面中

* 页面提交请求携带这个Token

* 服务器验证Token是否正确

总结：
总结下CSRF防御策略：

* CSRF自动防御策略：同源检测（Origin和Referer验证）
* CSRF主动防御：Token验证、双重Cookie以及Samesite Cookie
* 保证页面的幂等，不要在GET页面做用户操作

