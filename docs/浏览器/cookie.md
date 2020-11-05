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

* Expires
  + 会话性Cookie：用于设置Cookie的过期时间，当Cookie属性缺省时，表示会话性Cookie。当为会话性Cookie时，值保存在客户端内存中，关闭客户浏览器失效。

  + 持久性Cookie：与会话性Cookie相对应的是持久性Cookie。持久性Cookie存储在硬盘中，直至过期或者清除Cookie。设定的日期只和客户端有关，而不是服务端。

* Max-Age

  + 用于设定Cookie过期需要经过的秒数，Expires和Max-Age都存在，Max-Age优先级更高。
  + Max-Age为正数时，浏览器会持久化写入对应的Cookie文件中
  + Max-Age为负数时，表示Cookie是一次会话性Cookie
  + Max-Age为0时，表示删除这个Cookie

* Domain
  + Domain指定Cookie可以送达的主机名，假如没有指定，那么默认值为当前文档访问地址中的主机部分（不包含子域名）。

* path
  + 指定了一个URL，这个路径必须出现在要请求的资源的路径中才可以发送Cookie首部。

Domain和path共同定义了Cookie的作用域，即Cookie可以发送给哪些url。

* secure属性
  + 标记为secure的Cookie只应通过被https协议发送给服务端。使用https协议，可以保证Cookie不被篡改。

* httpOnly
  + 设置了httpOnly可以防止客户端脚本通过document.cookie等方式访问cookie，有助于避免xss攻击。

* sameSite: 可以让Cookie在跨站请求时不被发送，从而阻止跨站请求伪造攻击（CSRF）

  + 属性值
    - strict：仅允许一方请求携带cookie，即当前网页url与目标url完全一致允许

    - lax：允许部分第三方网站携带cookie

    - none：无论是否跨站都会发送cookie

  + 跨域和跨站
    - 跨站：两个URL的eTLD（有效顶级域名）+ 1即可。不考虑协议和端口。

* cookie的作用
  + 会话状态管理（用户登录、购物车等）
  + 个性化设置（自定义设置，主题等）
  + 浏览器行为跟踪（跟踪分析用户行为等）

* cookie缺点
  + cookie的数量和长度有限制，每个domain最多有20条cookie，每个cookie长度不能超过4kb，否则会被裁掉
  + cookie安全性问题
  + cookie增加http请求头大小
  + 没有封装好的getCookie和setCookie方法

* 获取Cookie

``` 
getCookie(name){
  let strCookie = document.cookie;
  let arrCookie = strCookie.split(';')
  for(let i = 0; i < arrCookie.length;i++){
    let str = arrCookie[i].split('=')
    if(name === str[0]){
      return str[1]
    }
  }
  return ''
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
