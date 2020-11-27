### Set

`Set` 本身是一个构造函数，用来生成 `Set` 数据结构。 `Set` 函数可以接收一个数组（或者具有iterable接口的其他数据结构）作为参数，用来初始化。

#### Set中的特殊值

`Set` 对象存储的值总是唯一的，所以需要判断两个值是否恒等。

* +0 === -0 // true, 只能存一个
* undefined === undefined // true, 只能存一个
* NaN === NaN //false, 但是在`Set`中认为相等，只能存一个

常见方法：
`[...new Set(array)]` // 数组去重 

`[...new Set('addda')].join('')` // 字符串去重

#### Set属性

`Set.prototype.contructor` : 构造函数，默认就是 `Set` 函数

`size` : 返回元素所包含的元素数量。

#### Set实例对象的方法

分为两大类：操作方法、遍历方法

##### 操作方法

`add` : 添加某个值，返回Set结构本身
`delete` : 删除某个值，返回一个boolean，表示删除是否成功
`has` : 返回布尔值，表示该值是否为Set成员
`clear` : 清除所有成员，没有返回值

在判断是否包含一个键上面， `object` 结构和 `Set` 结构写法不同。

``` 

const properties = {
  'width': 1,
  'height': 1
}

if(properties[Name]){}

Set 写法
const properties = new Set()

properties.add('width')
properties.add('height')

if(properties.has(Name)){}

```

`Array.form` 方法可以将Set结构转为数组。

const items = new Set([1, 2, 3])
const array = Array.from(items)

##### 遍历方法

`keys()` : 返回键名的遍历器
`Values` : 返回键值的遍历器
`entries` : 返回键值对的遍历器
`forEach` : 使用回调函数遍历每个成员

`keys` / `values` / `entries` 方法返回的都是遍历器对象，由于Set结构没有键名，只有键值(或者说键名就是键值)。所以keys和values方法行为完全一致。

Set结构的实例默认可遍历，它的默认遍历器生成函数就是它的values方法。这意味着，可以省略values方法，直接用for ... of遍历循环Set。

遍历的应用

* 扩展运算符(...): `[...new Set(arr)]`

* 数组`map`/`filter`: 数组map  filter可以间接的用于Set。

因此Set可以很容易实现并集(Union)/交集(intersect)和差集(difference)

let union = new Set([...a, ...b]) // 并集

let intersect = new Set([...a].filter(x => b.has(x))) // 交集

let difference = new Set([...a].filter(x => !b.has(x))) // 差集

### WeakSet

WeakSet结构和Set类似，也是不重复的值的集合。但是，与Set有两个区别。

* WeakSet成员只能是对象，不能是其他成员

* WeakSet中的对象都是弱引用，垃圾回收机制下不考虑WeakSet对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占的内存，不考虑该对象是否还在WeakSet中。 WeakSet成员是不适合引用的，因为它随时消失，由于WeakSet内部有多少成员取决于垃圾回收机制有没有运行，运行前后可能成员数量不一致，因此ES6规定WeakSet不可遍历。

#### 语法

 `const ws = new WeakSet()`

作为构造函数，WeakSet可以接收一个数组或者类似数组的对象作为参数。（实际上，任何具有Iterable接口的对象，都可以作为WeakSet的参数）。

WeakSet有三种方法：

* `add`: 向WeakSet实例添加一个新成员
* `delete`: 清除WeakSet实例的指定成员
* `has`: 返回一个布尔值

### Map

JS对象本质上是键值对的集合（Hash结构），但是传统上只能字符串当做键，这带来了很大的限制。

Map数据结构，是一种更完善的Hash结构实现，如果需要‘键值对’的数据结构，Map比Obejct更合适。

Map构造函数接收数组作为参数，任何具有Iterator接口、且每个成员都是一个双元素的数组的数据结构都可以当做是Map结构的参数。也就是说，Set和Map都可以用来生成新的Map。

如果对一个键多次赋值，后面的值将会覆盖前面的值。

如果读取一个未知的键返回undefined

注意，只有对同一个对象的引用，Map结构才将其视为同一个键。Map的键实际上是和内存地址绑定的，只要内存地址不一样，就视为两个键。

#### 实例的属性和方法

Map结构的实例有以下属性和方法

* `size`: 返回Map结构的成员总数

* `set`:`set`方法设置键名`key`对应的值为`value`，然后返回整个Map结构。如果key已经有值，则键值会被更新，否则就新生成该键。set方法返回的是当前的Map结构，因此可以采用链式写法。

* `get`方法获取`key`对应的键值，如果找不到`key`，返回`undefined`

* `has`方法返回一个布尔值，表示某个键是否在Map对象中

* `delete`方法删除某个键，返回true。如果删除失败，返回false

* `clear`方法清除所有成员，没有返回值。

Map结构原生提供三个遍历器生成函数和一个遍历方法

* key()：返回键名的遍历器
* values()：返回键值的遍历器
* entries()：返回所有成员的遍历器
* forEach()：遍历Map的所有成员

Map的遍历顺序就是插入顺序。

``` 

for(let [key, value] of map){  // 等同于map.entries
  console.log(key,value)
}
```

Map结构转为数组结构，比较快速的方法是使用扩展运算符(...)

``` 

[...map.keys()]  // [1,2,3]键名数组
```

``` 

[...map.values()]  // 键值数组
```

``` 

[...map.entries()]  // 键名键值二维数组
```

``` 

[...map]
```

结合数组的map、filter方法，可以实现Map的遍历和过滤（Map本身没有map和filter方法）。

与其他数据结构转换

* Map转为数组，使用扩展运算符(...)

``` 

[...myMap]

```

* 数组转为Map

``` 

new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])

```

* Map转为对象

``` 

function strMapToObj(strMap){
  let obj = Object.create(null);
  for (let [k,v] of strMap){
    obj[k] = v
  }
  return obj
}
```

* 对象转Map

可以通过 `Object.entries()`

也可以自己写个转换函数：

``` 

function objToStringMap(obj){
  let map = new Map()
  for(let k of Object.keys(obj)){
    strMap.set(k, obj[k])
  }
  return strMap
}
```

* Map转为JSON
* JSON转为Map

### WeakMap

WeakMap和Map的区别有两点：

* WeakMap只接收对象作为键名，不接受其它类型的值作为键名。
* WeakMap的键名所指向的对象 ，不计入垃圾回收机制

WeakMap设计目的在于，有时候我们想在某个对象上面存放一些数据，但是这会形成对于这个对象的引用。一旦不需要这个对象，就需要手动删除这个引用，否则垃圾回收机制就不会释放内存。

WeakMap就是解决这个问题诞生的，它的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占的内存。

注意：WeakMap弱引用只是键名，而不是键值。键值仍然是正常引用。

WeakMap与Map在Api上区别主要是两个，一个是没有遍历操作（keys, values, entries）, 也没有size属性。只有四个方法:get()/set()/has()/delete(); 
