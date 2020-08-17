### web worker

* 进程与线程的区别：进程与线程的概念以及单线程与多线程
* 浏览器内核（Rendring Engine）相关知识：GUI渲染线程，JavaScript引擎线程，事件触发线程等
* Web Worker是什么：Web Worker的限制与能力及主线程与Web Worker之间如何通信
* Web Worker的分类：Dedicated Worker、Shared Worker和Service Workers
* Web Worker API：Worker构造函数及如何观察Dedicated Worker等

#### 进程与线程的区别

进程和线程是对应CPU时间段的描述

* 进程（process）的概念：进程是指计算机中已运行的程序。包括上下文切换的程序执行时间的总和=CPU加载上下文 + CPU执行 + CPU保存上下文
* 线程（thread）的概念：操作系统中能够进行运算调度的最小单位。大部分情况被包含在进程之中，是进程中的实际运作单位。线程是共享了进程的上下文环境的更为细小的CPU时间段。
* 单线程与多线程：如果一个进程只有一个线程，称为单线程。单线程在程序执行的时候，所走的程序路径是按照顺序拍下来的，前面的处理好后面的才会执行。

#### 浏览器内核(Rendring Engine)

* GUI渲染线程

* JavaScript引擎线程

* 事件触发线程

* 定时器触发线程

* Http异步请求线程

#### web worker是什么

web worker是HTML5标准的一部分，这一规范定义了一套API，允许JS程序运行在主线程之外的另一个线程中。web worker的作用就是为JS创造多线程环境。

web worker的限制和能力：

不能直接操纵DOM元素，或使用window对象的某些方法或属性。

主线程与web worker之间的通信：

* `postMessage()` 方法来发送消息，通过 `onmessage` 这个事件处理器来接收消息。数据处理方式是传递副本，不是直接共享数据。

#### web worker的分类：

* Dedicated Worker专用线程
  + `let worker = new Worker('dw.js')`
  + `worker.onmessage = () => {}`
  + `worker.postMessage()`
  + 使用Blob URL创建inline Worker，Blob Url/Object Url是一种协议，允许Blob和File对象用作图像，下载二进制数据链接等的URL源。在浏览器中。使用URL.createObjecyURL方法来创建Blob URL，该方法接收一个Blbo对象，并为其创建一个唯一的URL。浏览器内为每个通过URL.createObjectURL生成的URL存储一个URL 到Blob映射，因此，此类URL较短，但可以访问Blob。生成的URL仅当前文档打开的状态下才有效。如果访问的Blob不存在，会从浏览器中收到404错误。

  ```const url = URL.createObjectURL(

    new Blob()

  )

``` 

* Shared Worker共享线程
  + 共享Worker是一种特殊类型的Worker，可以被多个浏览器上下文访问，比如多个window，iframe和workers，但是这些浏览器上下文必须同源。与常规Worker不同，我们需要使用 `onconnect` 方法等待连接。然后我们获得一个端口，该端口是我们与窗口之间的连接。
  + `let worker = new SharedWorker('shared-worker.js')`
  + `worker.port.start()`
  + `onconnect = (e) => { var port = e.ports[0]}  port.onmessage = () => {port.postMessage('aaaa')}`
* Shared Worker调试：
  + 可以通过 `chrome://inspect` 来进行调试

* Service Workers

Service Worker本质上充当Web应用程序与浏览器之间的代理服务器，也可以在网络可用时作为浏览器和网络间的代理。旨在创建有效的离线体验，拦截网络请求并基于网络是否可用以及更新的资源是否驻留在服务器上来才去适当的动作。

注意： 

* Service Worker运行在他们自己的完全独立异步的全局上下文中，也就是他们有自己的容器。
* Service Worker没有操作DOM的权限，可以通过postMessage方法来与Web页面通信，让页面操作DOM。
* Service Worker是一个可编程的网络代理，允许开发者控制页面上处理的网络请求
* 浏览器可以随时回收Service Worker，在不被使用的时候，他会自己终止。而当他再次被调用的时候，会被重新激活。

注册：
```
if ('serviceWorker' in navigator) {
    window.addEventListener('load', function () {
        // 所以Service Worker只是一个挂在navigator对象上的HTML5 API而已
        navigator.serviceWorker.register('/service-worker.js').then(function (registration) {
            console.log('我注册成功了');
        }, function (err) {
            console.log('我注册失败了');
        });
    });

}
```

* 我们需要手动编写service-worker.js文件
* 我们需要在网页中下载并注册service-worker.js文件
* Service Worker相当于网站的大脑，可以拦截并处理http请求

SW相当于网站的大脑，如果在 `http://www.example.com` 的根路径下注册了一个SW，那么这个SW可以控制所有该浏览器向 `http://www.example.com` 站点发起的请求。只需要箭筒fetch事件，就可以任意的操纵请求，可以从返回的CacheStorage中读取数据，也可以通过fetch API发起新的请求，甚至可以new一个Response，返回给页面。

SW生命周期：
* installing
* installed
* Activating
* Activated
* Redundant

首次导航到网站中，下载，解析并执行Service Worker文件，触发install事件，尝试安装Service Worker，如果install事件回调函数中的操作执行成功，标志Service Worker安装成功，此时进入waiting状态，注意这时Service Worker只是准备好，并没有生效。用户二次进入网站时，才会激活Service Worker，此时会触发activate事件，标志Service Worker正式启动。

Service Worker应用：
* 缓存静态资源：可以在install阶段，指定需要缓存的具体文件，在fetch事件的回调函数中，检查请求的url，如果匹配了已缓存的资源，就不再从服务端获取。适合js、css、字体、图片等静态资源。
* 离线体验



