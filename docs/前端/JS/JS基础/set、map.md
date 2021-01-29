### Set

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

数组去重 `[...new Set(array)]`
`Array.from()` 方法可以将Set结构转换成数组。

### Map 

类似对象也是键值对的集合，但是键的范围不限制于字符串，各种数据类型都能当做键。也就是说Object提供了“字符串-值”的对应，Map是“值-值”的对应。

Map结构有以下属性和操作方法：

* size属性

* set属性： `Map.prototype.set(key,value)` 返回整个Map结构，如果key已经有值，键值会更新，没有则新生成该键。

* `Map.prototype.get()`
* `Map.prototype.has()`
* `Map.prototype.delete()`
* `Map.prototype.clear()`


* Map.prototype.keys()：返回键名的遍历器。
* Map.prototype.values()：返回键值的遍历器。
* Map.prototype.entries()：返回所有成员的遍历器。
* Map.prototype.forEach()：遍历 Map 的所有成员。
