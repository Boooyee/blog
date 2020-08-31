### 编写loader和插件

 loader：一个导出为函数的JS模块

 多loader时的执行顺序：

  + 串行执行
  + 从右往左执行

函数组合的两种情况：
 + Unix的popline
 + Compose compose = (f, g) => (...args) => f(g(...args))

 
 
使用loader-runner调试
 + loader-runner允许不安装webpack的情况下运行loaders

 

``` 
 import { runLoaders } from 'loader-runner'

 runLoaders({
   resource: '/abs/path/to/file.txt?query',
   loaders: [
     {
       options:{
         name:'test'
       }
     }
   ],
   context:{},
   readResource:fs.readFile.bind(fs) 
 })

 ```

loader的参数获取：
 + 通过loader-utils的getOptions方法获取

``` 
const loaderUtils = require("loader-utils")

module.exports = function(content){
  const {name} = loaderUtils.getOptions(this)
}
```

loader异常处理：
 + loader内直接通过throw 抛出
 

``` 
 throw new Error('Error')
 ```

 + 通过this.callback传递错误
 

``` 
 this.callback(
   err:Error | null,  //error对象
   content: string | Buffer, //结果
   sourceMap?: SourceMap,
   meta?:any
 )
 ```

loader的异步处理：

* 通过this.async来返回一个异步函数。第一个参数是Error，第二个参数是处理的结果。

``` 
module.exports = function(input){

  const callback = this.async()

  callback(null,input + input)
}
```

在loader中使用缓存：

* webpack中默认开启loader缓存
  + 可以使用this.cacheable(false)关掉缓存
* 缓存条件：loader的结果在相同的输入下有确定的输出
  + 有依赖的loader无法使用缓存


loader如何进行文件输出：通过this.emitFile进行文件写入。

```
const loaderUtils = require('loader-utils')

module.exports = function(content){
  const url = loaderUtils.interpolateName(this,"[hash].[ext]",{
    content
  })
  this.emitFile(url,content)

  const path = `_webpack_public+path_+${JSON.stringify(url)}`;  // webpack全局变量

  return `export default ${path}`
}
```

### 插件

插件没有没有像loader那样独立的运行环境，只能在webpack内运行。


