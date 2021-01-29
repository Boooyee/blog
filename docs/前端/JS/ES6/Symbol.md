Symbol特点：

* Symbol()是一个函数，有一个可选的字符串参数。不允许作为构造器使用，即不允许使用new
* 返回的是原始数据类型Symbol，instanceof的值为false
* 如果参数是一个对象，就会调用该参数的toString方法，将其转换成字符串然后才生成Symbol。
* 具有唯一性
* 不允许隐式转换成字符串，Symbol值可以显示的转换为字符串
* 不允许转换成数字
* 有两个静态方法for和keyFor
  + 如果使用同一个Symbol值，可以使用Symbol.for。它接收一个字符串作为参数，然后搜索有没有以该参数作为名称的Symbol值，
  + Symbol.keyFor返回一个已登记的Symbol类型值的key
* Symbol作为属性名，该属性不会出现在for in, for of循环中, 也不会被Object.keys、Object.getOwnPropertyNames()、JSON.stringify()返回。但是他也不是一个私有属性，

``` 
typeof Symbol.for  // function
typeof Symbol.keyFor // function
```

接下来根据规范实现Polyfill，分为四个部分：

* 构造器
* 构造器属性
* prototype属性
* 实例的属性

构造器：

Symbol作为构造器

``` 
 (function () {
      var root = this;

      var generateName = (function () {
        var postfix = 0;
        return function (descString) {
          postfix++;
          return '@@' + descString + '_' + postfix
        }
      })()

      var SymbolPolyfill = function Symbol(description) {

        if (this instanceof SymbolPolyfill) throw new TypeError('Symbol is not a constructor');

        var descString = description === undefined ? undefined : String(description)

        var symbol = Object.create({
          toString: function () {
            return this.__Name__;
          }
        })

        Object.defineProperties(symbol, {
          '__Description__': {
            value: descString,
            writable: false,
            enumerable: false,
            configurable: false
          },
          '__Name__': {
            value: generateName(descString),
            writable: false,
            enumerable: false,
            configurable: false
          }
        });

        return symbol;
      }

      root.SymbolPolyfill = SymbolPolyfill;

    })()
```
