## 封装ajax

使用XMLHttpRequest对象进行请求发送和响应,主要步骤：
1、创建异步XMLHttpReqest对象：`let request = new XMLHttpRequest()`

2、配置参数（设置请求基本参数）：`request.open('method','path')`

3、发送请求：`request.send()`

4、注册onreadystatchange监听事件，只要状态改变就会调用，监听XMLHttpRequest对象：`request.onreadystatchange()`

onreadystatchange用来监听readyState状态值。

| 状态值 | 描述|
| ---- | ---- |
| 0 | 请求还未初始化，还未调用open() |
| 1 | 请求已建立但未发送，还未调用send() |
| 2| 接受原始响应数据，为解析做准备|
| 3| 正在解析数据,根据响应头部返回的MIME类型把数据转换成能通过responseText等形式存取的格式|
|4|响应完成，数据解析完成|

## 具体步骤

发送请求：
1、请求行（请求方法，请求路径，协议）：`request.open('method','/path')`

2、请求头：`request.setHeader('header',value)`

3、请求体：`request.send('data')`

响应请求：

1、响应行（协议版本，响应码，响应文本）：`request.state` `request.statusText`

2、响应头：`request.getResponseHeader()` `request.getAllResponseHeaders()`

3、响应体：`request.responseText`

fetch：
fetch是浏览器提供的原生AJAX接口，基于promise设计，挂在BOM上，属于全局方法
```
fetch(...).then(fun2)
          .then(fun3)
          .catch(fun)
```

fetch发送请求时，不像XHR默认带上cookie，需要我们手动设置带上Cookie`credentials: 'include'`
```
fetch('http://www.xxx.com',{
  method:'get',
  credentials: 'include'
})
```
XMLHttpReqest存在的问题：
1、设计上不符合职责分离原则，将输入，输出，事件回调混在一个对象中。
2、基于事件模型设计，存在回调地狱问题。

fetch优缺点：

1、基于Promise，解决回调地狱问题，ES规范里的实现方式

2、使用起来简洁

3、默认不带cookie，需要手动添加

4、浏览器支持不友好，需要第三方ployfill

5、只对http请求报错，对400，500都当做成功的请求，需要封装去处理

6、不支持abort，不支持超时控制，使用setTimeout以及Promise.reject实现的超时控制不能阻止请求过程继续在后台执行

7、没有办法原生监听请求的进度，XHR可以

