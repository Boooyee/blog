### 自动清理构建目录

clear-webpack-plugin

``` 
plugins: [
  new CleanWebpackPlugin()
]
```

### CSS3属性前缀

postcss-loader

aotuprefixer自动补齐css3前缀（后置处理）

``` 
rules:{
  test:/\.less$/,
  use:[
    'style-loader',
    'css-loader',
    'less-loader',
    {
      loader: 'postcss-loader',
      options: {
        plugin: [
          () => {
            require('autoprefier')({
              browsers: ['last 2 version']
            })

          }
        ]
      }
    }
  ]
}
```

### 移动端css px自动转换成rem

rem：font-size of the root element

1、移动端css px自动转换成rem，使用px2rem-loader

2、页面渲染时计算根元素font-size值，可以使用手淘的lib-flexible库

``` 
rules: [
  {
    test:/\.less$/,
    use: [
      'style-loader',
      'css-loader',
      'less-loader',
      {
        loader: 'px2rem-loader',
        options: {
          remUnit:75,
          remPrecision:8
        }
      }
    ]
  }
]
```

### 静态资源内联

代码层面：
 + 页面框架初始化脚本
 + 上报相关大点
 + css内联避免页面闪动（首屏css内联）

 请求层面：减少HTTP网络请求

  + 小图片或者字体内联（url-loader）

HTML和JS内联：raw-loader

CSS内联：

* style-loader

``` 
module.exports = {
  module:{
    rules:[
      {
        test: /\.scss$/,
        use:[
          {
            loader:'style-loader',
            options:{
              inserAt: 'top',  //插入样式到header
              singleton:true  // 将所有的style标签合并成一个
            }
          },
          'css-loader',
          'sass-loader'
        ]
      }
    ]
  }
}
```

* html-inline-css-webpack-plugin

### source map

通过source map定位到源代码

开发环境开启，线上环境关闭

### 提取相同模块

基础库分离：react react-dom通过cdn引入，不打入bundle中。

SplitChunksPlugin:进行脚本分离

chunks参数说明：
 + async 异步引入的库进行分离
 + initial：同步引入的库进行分离
 + all：所有引入的库进行分离

minChunks：设置最小引用次数
minuSize：分离包的体积大小

### tree shaking（摇树优化webpack4默认开启）


webpack默认支持，在.babelrc里设置modules:false即可，production mode默认开启。必须是ES6语法，require不支持。

原理：利用ES6模块特点：
 + 只能作为模块顶层出现
 + import的模块名只能是字符串常量（不能动态require）
 + import binding 是immutable


### Scope Hosting

原理：将所有的代码按照引用顺序放在一个函数作用域里，然后适当的重命名一些变量以防止变量名冲突。

通过scope hoisting可以减少函数声明代码和内存开销

scope hoisting使用：在production中默认开启，必须是ES6语法，CJS不支持。

### 代码分割动态import

意义：减少首屏加载时间。webpack有一个功能是将你的代码库分割成chunks，当代吗运行到需要他们的时候再进行加载。

使用场景：
  + 抽离相同代码到一个共享块
  + 脚本懒加载，使得初始下载的代码更小

懒加载JS脚本的方式：
 + CommonJS：require.ensure
 + ES6：动态import（还没有原生支持，需要babel转换 ）
   + 安装babel插件 npm i @babel/plugin-syntax-dynamic-import --save-dev
   + 添加到.babelrc文件中
   ```
      {
        "plugins": ["@babel/plugin-syntax-dynamic-import"]
      }
     ```

### webpack里使用ESLint

优秀实践：1、eslint-config-airbnb 2、eslint-config-airbnb-base

指定团队的ESLint规范：
 + 不重复造轮子，基于eslint:recommend配置并改进
 + 能够帮助发现代码错误的规则，全部开启
 + 帮助团队代码风格统一，而不是限制开发体验

ESLint如何落地：
 + 和CI/CD集成
 + 和webpack集成：使用eslint-loader，构建时检查js规范
 ```
 module.exports = {
   module:{
     rules: [
       {
         test:/\.js$/,
         exclude:/node_modules/,
         use: [
           "babel-loader",
           "eslint-loader"
         ]
       }
     ]
   }
 }
 ```

引入ESLint可以在package.json或者在.eslintrc.js文件中配置

 













