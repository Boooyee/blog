### Cookie

某些网站为了辨别用户身份而存储在用户本地终端上的数据，一般一段不超过4kb的小型文本数据，由一个名称（Name）一个值（Value）和其他几个用于控制Cookie有效期、安全性、使用范围和可选属性组成。

* Cookie的查看

Cookie可以在浏览器上查看，存储在本地文件里。

#### Cookie的设置

* 客户端发送http请求到服务器
* 服务器收到http请求时，响应头添加一个Set-Cookie字段
* 浏览器接收到响应后保存起来
* 之后对该服务器每一次请求中都通过Cookie字段将Cookie信息发给服务器

#### Cookie的属性

* names/value
  + 用JS操作Cookie注意对value进行编码处理

* Expires：用于设置Cookie的过期时间。

  + 会话性Cookie：当Expires属性缺省时，表示会话性Cookie。当为会话性Cookie时，值保存在客户端内存中，关闭客户浏览器失效。

  + 持久性Cookie：与会话性Cookie相对应的是持久性Cookie。持久性Cookie存储在硬盘中，直至过期或者清除Cookie。设定的日期只和客户端有关，而不是服务端。

* Max-Age：(用于设定Cookie过期需要经过的秒数)

  + Expires和Max-Age都存在，Max-Age优先级更高。
  + Max-Age为正数时，浏览器会持久化写入对应的Cookie文件中
  + Max-Age为负数时，表示Cookie是一次会话性Cookie
  + Max-Age为0时，表示删除这个Cookie

* Domain
  + Domain指定Cookie可以送达的主机名，假如没有指定，那么默认值为当前文档访问地址中的主机部分（不包含子域名）。不能跨域设置Cookie，比如阿里域名下的页面把Domain设置成百度是无效的。

* path
  + 指定了一个URL，这个路径必须出现在要请求的资源的路径中才可以发送Cookie首部。

Domain和path共同定义了Cookie的作用域，即Cookie可以发送给哪些url。

* secure属性
  + 标记为secure的Cookie只应通过被https协议发送给服务端。使用https协议，可以保证Cookie不被窃取和篡改。

* httpOnly
  + 设置了httpOnly可以防止客户端脚本通过document.cookie等方式访问cookie，有助于避免xss攻击。

* sameSite: 可以让Cookie在跨站请求时不被发送，从而阻止跨站请求伪造攻击（CSRF）

  + 属性值
    - strict：仅允许一方请求携带cookie，即当前网页url与目标url完全一致允许

    - lax：允许部分第三方网站携带cookie

    - none：无论是否跨站都会发送cookie

  + 跨域和跨站
    - 跨站：两个URL的eTLD（有效顶级域名）+ 1即可。不考虑协议和端口。

  + 从None改成Lax影响：
    - post表单：发送Cookie > 不发送Cookie
    - iframe：发送Cookie > 不发送Cookie
    - Ajax：发送Cookie > 不发送Cookie
    - image：发送Cookie > 不发送Cookie

* cookie的作用
  + 会话状态管理（用户登录、购物车等）
  + 个性化设置（自定义设置，主题等）
  + 浏览器行为跟踪（跟踪分析用户行为等）

* cookie缺点
  + cookie的数量和长度有限制，每个domain最多有20条cookie，每个cookie长度不能超过4kb，否则会被裁掉
  + cookie安全性问题
  + cookie增加http请求头大小
  + 没有封装好的getCookie和setCookie方法

* Cookie和CSRF的关系

CSRF：跨站请求伪造，发生的场景就是，用户登录了a网站，然后跳转到b网站，b网站直接发送一个a网站的请求，进行了一些危险的操作，就发生了CSRF攻击。

注意：为什么在b网站可以仿造a网站的请求，Cookie不是跨域的吗，什么条件下，什么场景下会发生这样的事情？

这时候，需要注意对Cookie的定义，在发送一个http请求时，携带的Cookie是这个http请求域的Cookie。也就是在b网站，发送a网站的一个请求，携带的是a网站域名下的cookie。注意不是：b网站发送任意一个请求，只能携带b网站域名下的cookie。

当然，在b网站下，读取cookie的时候，只能读取b网站域名下的cookie，这是cookie的跨域限制。所以要记住，不要把http请求携带的cookie，和当前域名访问权限的cookie混淆在一起。

* Cookie相关特性：
  + http请求，会自动携带Cookie。
  + 携带的Cookie，还是http请求所在域名的Cookie

* Cookie如何应对CSRF攻击

  + 放弃Cookie，使用token：Token策略，一般就是登陆的时候，服务端在response中，返回一个token字段，然后以后所有的通信，前端就把这个token添加到http请求的头部。

  + sameSite Cookies：Cookie有一个属性，能够解决CSRF攻击问题：sameSite。它表示，只能当前域名的网站发出的http请求，携带这个Cookie。由于这个是新Cookie属性，兼容性上有问题。

  + 服务端Referer验证：我们发送的http请求，header中会携带Referer字段，这个字段代表的是当前域的域名，服务端可以通过这个字段来判断，是不是真正的用户请求。这个方案用的比较少，Referer字段也可能伪造。

* Cookie与XSS攻击

  + XSS是由于不安全数据引起的，有可能是表单提交的数据，有可能是页面路径的参数问题。XSS是通过窃取用户的敏感信息而达到攻击的目的，比如本地存储，用户密码，Cookie等。

  + 比如这个不安全的数据，是一个script标签，那这个script就可以链接任意的js文件，浏览器本地就会执行这个js，那么通过js代码可能：1、通过document.cookie，获取用户信息  2、通过localStorage，获取本地存储的敏感信息

  + Cookie应对XSS攻击

    - 方案1：http-only：设置http-only，表示Cookie只能被http请求携带
    - 方案2：正则校验：对表单提交数据进行正则校验，校验通过后，才能提交数据
    - 方案3：数据转义：如果无法保证数据库的数据安全，把页面数据进行转义，比如script，<>等

  + Cookie和Token进行对比：

    - Cookie可能引起csrf攻击，token在保持用户会话会好一点
    - http请求携带Cookie，当Cookie过大的时候，会增大http请求的带宽
    - Cookie的特性导致Cookie面对SCRF攻击不安全

  + Cookie如何做优化
    - 安全方面，使用Token进行会话保持

    - http角度，尽可能让Cookie信息少一点，从而使http请求的体积更小

    - Cookie的作用是保持用户会话，所以仅仅在接口请求的时候使用Cookie
    - 加载其他资源，比如图片、JS、CSS文件等可以托管到CDN上，这样就不会携带Cookie，CDN的策略使得资源加载更快

  

    

* 获取Cookie

``` 

function getCookie(name) {
      let value = `; ${document.cookie}`
      let parts = value.split(`; ${name}=`)
      if (parts.length === 2) {
        return parts.pop().split(';').shift()
      } else {
        return ''
      }
    }
```

设置cookie

``` 

function addCookie(key, value, day, path, domain) {
  // 1.处理默认保存的路径
  var index = window.location.pathname.lastIndexOf("/")
  var currentPath = window.location.pathname.slice(0, index);
  path = path || currentPath;
  // 2.处理默认保存的domain
  domain = domain || document.domain;
  // 3.处理默认的过期时间
  if (!day) {
    document.cookie = key + "=" + value + ";path=" + path + ";domain=" + domain + ";";
  } else {
    var date = new Date();
    date.setDate(date.getDate() + day);
    document.cookie = key + "=" + value + ";expires=" + date.toGMTString() + ";path=" + path + ";domain=" + domain + ";";
  }
}
```


### 最新Cookie属性（Chrome 87）

