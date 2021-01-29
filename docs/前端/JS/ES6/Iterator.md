### Iterator(遍历器)

JS中表示集合的数据结构，主要是数组和对象，ES6中添加了Map和Set，这样就有了四种数据集合。

Iterator是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署了iterator接口，就可以完成遍历操作（依次处理该数据结构的所有成员）。

Iterator作用有三个：

* 为各种数据结构提供一个统一的、简便的访问入口
* 使得数据结构的成员能够按照某种次序排列
* ES6创造了一种新的遍历命令`for  of`, Iterator接口主要供`for of`消费

Iterator遍历过程

* 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象
* 第一次调用指针的next方法，可以将指针指向数据结构的第一个成员
* 第二次调用指针的next方法，指针指向数据结构的第二个成员

* 不断的调用指针的next方法，直到它指向数据结构的结束位置

模拟next例子

``` 

let it = markeIterator(['a','b'])

function markeIterator(array){
  let nextIndex = 0
  return {
    next:() => {
      return nextIndex < array.length ? 
      { value: array[nextIndex++], done: false} :
      { value: undefined, done: true}
    }
  }
}
```

#### 默认Iterator接口

默认Iterator接口：Iterator接口的目的，就是为所有数据结构，提供一种统一的访问机制，即 `for of` 循环。当 `for of` 循环遍历某种数据结构时，该循环会自动去寻找Iterator接口。

* ES6规定，默认的Iterator接口部署在数据结构的`Symbol.iterator`属性，一个数据结构只要具有`Symbol.iterator`就可以认为是可遍历的。`Symbol.iterator`属性本身是个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。至于属性名`Symbol.iterator`, 它是一个表达式，返回`Symbol`对象的`iterator`属性，这是一个预定义好的，类型为Symbol的特殊值。

``` 

const obj = {
  [Symbol.iterator] : () => {
    return {
      next: () => {
        return {
          value: 1,
          done: true
        }
      }
    }
  }
}
```

上面代码中，对象obj是可遍历的（iterable）, 因为具有 `Symbol.iterator` 属性。执行这个属性，会返回一个遍历器对象。该对象的根本特征就是具有next方法。每次调用next方法，都会返回一个代表当前成员的信息对象，具有value和done属性。

ES6的有些数据结构原生具有Iterator接口，即不用任何处理，就可以被 `for of` 循环遍历。原因在于，这些数据结构原生部署了Symbol.iterator属性，另外一些数据结构没有(比如对象)。凡是部署了Symbol.iterator属性的数据，就称为部署了遍历器接口。调用这个接口，就会返回一个遍历器对象。

原生具备Iterator接口的数据结构如下：

* Array
* Map
* Set
* String
* TypedArray
* 函数的arguments
* NodeList对象

对于原生部署Iterator接口数据结构，不用自己写遍历器生成函数，for  of循环会自动遍历他们。除此之外，其他数据结构（主要是对象）的Iterator接口，都需要自己在Symbol.iterator属性上部署，这样才会被for of循环。

对象之所有没有默认部署Iterator接口的数据结构，因为对象哪个属性先遍历，哪个属性后遍历是不确定的，需要开发者手动指定。本质上，遍历器是一种线性处理，对于任何非线性数据结构，部署遍历器接口，就等于部署一种线性转换。不过，严格来说，对象部署遍历器接口并不是很必要，因为这时候对象实际上被当做是Map数据结构使用，ES5没有Map结构，ES6提供了。

#### 调用Iterator接口的场合

* 解构赋值：对数组和Set结构进行结构赋值时，会默认调用Symbol.iterator方法。

* 扩展运算符：扩展运算符也会调用默认的Iterator接口

* yield*： yield*后面跟的是一个可遍历结构，它会调用该结构的遍历器接口

* 其它场合：由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口
  + for of
  + Array.from
  + Map() Set() WeakMap() WeakSet()
  + Promise.all
  + Promise.race

#### 字符串的Iterator接口

字符串是个类数组的对象，也原生具有Iterator接口。

``` 

let someString = 'hi'
typeof someString[Symbol.iterator]  // 'function'

let iterator = someString[Symbol.iterator]()

iterator.next();

iterator.next();

iterator.next()
```

上面代码中，调用 `Symbol.iterator` 方法返回一个遍历器对象，在这个遍历器对象上可以调用next方法，实现对于字符串的遍历。

可以覆盖原生的 `Symbol.iterator` 方法，达到修改遍历器行为的目的。

``` 

var str = new String('hi')

[...str]  // ['h','i']

str[Symbol.iterator] = function(){
  return {
    next: () => {
      if(this._first){
        this._first = false;
        return {
          value: 'bye', done: false
        } else {
          return { done : true}
        }
      },

      _first { done : true}
    }
  }
}

[...str] // ['bye']
str // hi
```

#### Iterator 接口与Generator函数

`Symbol.iterator()` 方法的最简单实现，是使用 `Generator` 函数。

``` 

let MyIterator = {
  [Symbol.iterator]: function*(){
    yield 1;
    yield 2;
    yield 3;
  }
}
```

``` 

let obj = {
  *[Symbol.iterator](){
    yield 'hello';
    yield 'world'
  }
}

for(let x of obj){
  console.log(x)
}

```

#### 遍历器对象的return{}  throw{}

遍历器对象除了具有 `next()` 方法，还可以具有 `return()` 方法和 `throw()` 方法。如果自己写遍历器对象生成函数，那么 `next()` 方法是必须部署的， `return()` 和 `throw()` 方法是否部署是可选的。

`return()` 方法的使用场景是，如果 `for of` 循环提前退出（报错或者break语句），就会调用return()方法。如果一个对象完成遍历前，需要清理或者释放资源，就可以部署return()方法。

### for of 

for of作为遍历所有数据结构的统一方法，一个数据结构只要部署了 `Symbol.iterator` 属性，就被视为具有Iterator接口，就可以用for of循环遍历它的成员。也就是说，for of循环内部调用的是数据结构的 `Symbol.iterator` 方法。

for of循环可以使用范围包括：数组、Set和Map结构、某些类似数组的对象(argument、DOM NodeList对象)、后文的Genarator对象、字符串。

#### 数组

数组原生具备了iterator接口（默认部署了Symbol.iterator属性），for of循环本质上是调用这个接口产生遍历器。

``` 

const arr = ['red', 'green', 'blue']

for(let v of arr){
  console.log(v)
}

const obj = {}
obj[Symbol.iterator] = arr[Symbol.iterator],bind(arr)

for(let v of obj){
  console.log(v)
}
```

#### Set和Map结构

Set和Map结构也原生具有iterator接口，可以使用for of循环、

* 遍历的顺序是各个成员被添加到数据结构的顺序
* Set结构遍历时返回的是一个值，Map结构遍历时，返回的是一个数组，该数组的两个成员分别是当前Map成员的键名和键值

#### 计算生成的数据结构

有些数据结构是在现有数据结构的基础上，计算生成的。比如，ES6数组，Set，Map都部署一下三个方法，调用后返回遍历器对象。

* entries(): 返回一个遍历器对象，用来遍历[键名, 键值]组成的数组。
* keys(): 返回一个遍历器对象，用来遍历所有的键名
* values(): 返回一个遍历器对象，用来遍历所有键值

#### 类数组对象

类数组对象包括好几类，字符串，DOM NodeList对象，argument对象。

* 字符串，for of能正确识别32位 UTF-16字符
* 并不是所有的类数组对象都有Iterator接口，一个简单的解决方法就是Array.from转换成数组

#### 对象

对于普通对象，for of结构不能直接使用，会报错，必须部署了Iterator接口后才能使用。但是，这样情况下，for in 循环仍然能遍历键名。

对于普通对象，for in循环可以遍历键名，for of循环会报错。

* 一种解决方法是使用Object.keys方法将对象键名生成一个数组，然后遍历这个数组
* 另一种思路是Generator函数将对象重新包装下

#### 与其他遍历语法比较

* for循环：写法麻烦
* forEach: 无法中途跳出循环，break或者return不能奏效

* for in 可以遍历数组的键名，缺点如下
  + 数组的键名是数字，但是for in 循环十一字符串作为键名
  + for in 不仅仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型上的键
  + 某些情况下，for in会以任意顺序遍历键名

for in 主要是为遍历对象而设计的，不适用于遍历数组

for of循环有一些显著优点

* 同for in写法一样简洁，没有for in缺点

* 不同于forEach，可以和break、continue、return配合
* 提供了遍历所有数据结构的统一操作接口



