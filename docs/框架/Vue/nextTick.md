### nextTick实现原理

* 将异步操作函数暂存起来

* 通过pending防止重复调用异步函数（类似防抖/节流）

* 对异步进行兼容性处理
  + Promise
  + Mutationobserver
  + setImmediate：非标准特性，把一些需要长时间运行的操作放在一个回调函数中，浏览器完成后面的其他语句，立即执行这个回调函数。
  + setTimeout

#### Vue数据异步更新机制Vue.nextTick

浏览器从服务器请求静态资源到页面显示出来，大致分为五个步骤：

* HTML转换为DOM树
* CSS转换为CSS规则
* 结合DOM树和CSS规则构建渲染树

* 在内存中布局，计算出各个元素的大小，颜色，位置等
* 把布局绘制到屏幕上

减少重排（Layout）和重绘（Painting）：

* Vue把DOM更新设计为异步更新，每次侦听到数据变化，将开启一个队列，并缓存在同一事件循环中发生的所有数据变更。如果同一个watcher被多次触发，只会被推入到队列中一次。然后再下一次事件循环tick中，Vue才会真正执行队列中数据变更，然后页面才会重新渲染。相当于多个地方的DOM更新放到一个地方一次性全部更新。
