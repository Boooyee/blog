### for in

* for in 
  - 应用于数组循环时，返回的是数组的下标和数组的属性和原型上的方法和属性。
  - 应用于对象循环时返回的是对象的属性名和原型中的方法和属性

* for in遍历存在的问题
  - index索引为字符串型数字，不能直接用于几何运算
  - 遍历顺序有可能不是按照实际数组的内部顺序
  - 使用`for in`会遍历数组所有的可枚举属性，包括原型。


### for of




