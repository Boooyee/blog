### 对比差异

- computed是计算一个新的属性，并将该属性挂载到Vue实例上，Vue监听已经存在并且挂载到Vue上的数据

- computed本质是个惰性的观察者，具有缓存行，只有依赖发生变化后，第一次访问computed属性，才会计算新值，watch是数据发生变化就执行函数

- 使用场景上，computed适合一个数据被多个数据影响，watch适合一个数据影响多个数据