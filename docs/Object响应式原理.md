## 追踪变化
首先定义一个响应式数据

``` 
  function definedReactive(data, key, value) {
      Object.defineProperty(data, key, {
        enumerable: true,
        configurable: true,
        get() {
          return value
        },
        set(newVal) {
          if (value === newVal) {
            return;
          }
          console.log('监听到值变化了 ', value, ' --> ', newVal);
          value = newVal
        }
      })
    }
```

## 收集依赖

之所以要观察数据是因为要在数据发生变化后通知到使用数据的地方。所以我们先收集依赖，然后在数据发生变化时把收集到的依赖循环触发一遍。总结起来就是在getter中收集依赖，在setter中触发依赖。就是实现一个消息订阅器，在getter中收集订阅者，在setter中通知订阅者。

``` 

```

## watcher

我们要通知用到数据的地方可能很多，而且类型还不一样。所以需要抽象一个类，需要它1、在自身实例化时往属性订阅器（dep）添加自身2、属性变动dep.notify()时调用自身update()方法，再通知其他地方。

## 递归侦测所有key

封装一个Observer类，这个类作用是把一个数据内所有key都转换成getter/setter形式，然后去追踪他们的变化。

``` 
class Observer {
  constructor(value) {
    this.value = value
      if (!Array.isArray(value)) {
        this.walk(value)
      }
    }
    walk(obj) {
      const keys = Object.keys(obj)
      for (let i = 0; i < keys.length; i++) {
        defineReactive(obj, keys[i], obj[keys[i]])
      }
    }
  }
```
