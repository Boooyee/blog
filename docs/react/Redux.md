### Redux

#### 使用Redux 原因，Redux的三个特性

Redux: 让组件通信更加容易。

特性：

* `Single Source of Truth`，所有状态放在唯一的store中

* 可预测性：`state + action = new state`

* 纯函数更新Store（产生新的Store）
  + 纯函数：输出结果完全取决输入参数

#### 理解Store、Action、Reducer

* Store：`const store = createStore(reducer)`
  + getState()
  + dispatch(action)
  + subscribe(listener)  //监听store变化

* action: 描述行为的一个记录

* reducer：是个函数，接收`state`和`action`

* `combineReducers`: 接收多个Reducer作为参数

  + 
  
* `bindActionCreators`：工具函数
  + `plusOne = bindActionCreators(plusOne,store.dispatch)`，封装`dispatch`不需要知道store和dispatch

 
