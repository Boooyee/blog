### this

this关键字执行为当前执行环境的ThisBinding。

* 全局上下文this指向全局对象，浏览器中this指向window。

* 函数上下文，this取决于函数调用方式
  + 直接调用指向window
  + call、apply指向绑定的对象上
  + bind绑定到bind的第一个参数上
  + 箭头函数，没有自己的this，指向最外层
  + 作为对象的方法
  + 作为构造函数：this被绑定在正在构造的新对象

* call/apply/bind的模拟实现

``` 
 Function.prototype.myBind = function (oThis) {
      let self = this
      let args = [].slice.call(arguments, 1)

      var fNOP = function () { };

      let fBound = function () {
        var bindArgs = [].slice.call(arguments)
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs))
      }

      fNOP.prototype = this.prototype;
      fBound.prototype = new fNOP();
      return fBound
    }
```
