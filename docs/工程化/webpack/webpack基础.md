### webpack基本概念

* Entry：用来指定打包入口
  + 单入口：entry是一个字符串
  + 多入口：entry是一个对象

* output：用来告诉webpack如何将编译后的文件输出到磁盘

``` 
output:{
  filename: '[name].js',
  path:path.join(_dirname, '/dist')
}
```

* Loaders:webpack开箱只支持JS和JSON两种文件类型，通过loaders去支持其他文件类型并把它们转换成有效的模块，并且添加到依赖树中。本身是一个函数，接收源文件作为参数，返回转换的结果。

常用的loader：babel-loader、css-loader、less-loader、ts-loader、file-loader、raw-loader、thread-loader.

loaders用法：

``` 
module:{
  rules:[
    {test: /\.txt$/, use: 'raw-loader'}
  ]
}
```

* plugins：用于bundle文件的优化，资源管理和环境变量的注入，作用域整个构建过程。

常用的Plugins：
 + CommonsChunkPlugin: 将chunks相同的模块代码提取成公共js
 + CleanWebpackPlugin：清理构建目录
 + ZipWebpackPlugin: 将打包出来资源生成一个zip包
 + ExtractTextWebpackPlugin：将css 从bundle文件里提取成一个独立的css文件
 + CopyWebpackPlugin: 将文件或者文件夹拷贝到构建的输出目录
 + HtmlWebpackPlugin: 创建Html文件去承载输出的bundle

* Mode:webpack4提出来的，用来指定当前的构建环境：production/development/none。Mode优点就是开启内置函数功能。

Mode内置函数：
 + development：process.env. NODE_ENV值为development，开启NamedChunksPlugin和NamedModulesPlugin
 + production：设置process.env. NODE_ENV值为production
 + none：不开启任何优化项

### 解析ES6

* babel-loader，需要创建 `.babelrc` 文件。解析React JSX需要在presets新增 `@babel/preset-react`

``` 
{
  "presets": [
    "@babel/preset-env"
  ],
  "plugins": [
    "@babel/proposal-class-properties"
  ]

}
```

### 解析css

* 需要css-loader，用于加载 `.css` 文件，并且转换成commonjs对象。
* style-loader将样式通过<style>标签插入到head中。

### 解析图片、字体

file-loader：处理字体、图片

``` 
rules: [
  {
    test: /.(png|jpg|gif|jpeg)$/,
    use: 'file-loader'
  },
  {
    test: /.(woff|woff2|eot|ttf|otf)$/,
    use: 'file-loader'
  }
]
```

### webpack中文件监听

文件监听是在发现源码发生变化时，自动构建出新的输出文件。

webpack开启监听模式，有两种方式：

* 启动webpack命令时，带上--watch参数
* 在配置webpack.config.js中设置watch:true

文件监听原理分析：

* 轮询

轮询判断文件的最后编辑时间是否变化

某个文件发生变化，并不会立即告诉监听者，而是先缓存起来，等aggregate Timeout。

``` 
module.export = {
  watch: true,
  watchOptions:{
    ignored: /node_modules/,
    aggregate Timeout: 300,
    poll:1000
  }
}
```

### 热更新 webpack-dev-server

WDS：不刷新浏览器

WDS：不输出文件，而是放在内存中

使用HotModuleReplacementPlugin插件

也可以使用webpack-dev-middleware，WDM将webpack输出的文件传输给服务器，适用于灵活的定制场景。

* 热更新原理实现

 + 基本概念：
   + webpack Compiler：webpack编译器将js源代码编译成bundle文件。
   + HMR Server将热更新文件输出给HMR Runtime
   + Bundle server：提供文件在浏览器的访问
   + bundle.js构建输出的文件
   + HMR Runtime；会被注入到浏览器，更新文件的变化，通常是websocket连接。
 + 过程
   + 启动阶段：文件系统 > 通过Webpack Compiler > (编译好的文件)传输给Bundle Server(让这个文件以server的方式让浏览器访问到)
   + 文件更新：文件系统 > webpack compiler >(编译好之后发送给)HMR Server (HMS Server知道哪些文件发生了改变)(服务端)> (通知)HMR Runtime(在客户端浏览器)

### 文件指纹

文件指纹：打包输出的文件名的后缀。做版本的管理。

Hash：和整个项目的构建相关，只要项目文件有修改，整个项目构建的hash就会变化。

Chunkhash：和webpack打包的chunk有关，不同的entry会生成不同的Chunkhash值

Contenthash：根据文件内容来定义hash，文件内容不变则Contenthash不变。有js和css文件，如果都用Chunkhashjs改变了但css没变的话css hash值也会改变，因此根据文件内容css通常用Contenthash值。

* js文件指纹设置: 设置 `output` 的 `filename` ，使用[chunkhash]

* css的文件指纹设置：
  + 设置`MiniCssExtractPlugin`的filename，因为使用`style-loader`会将css插入head头部，但是并没有形成一个文件，因此需要使用这个插件。
  + 使用[contenthash]
  ```
  plugins: [
    new MiniCssExtractPlugin({
      filename: `[name][contenthash:8].css`
    })
  ]

  ```

* 图片的文件指纹设置

设置file-loader的name，使用hash。

### 代码压缩

* JS压缩：webpack4默认设置了 uglifyjs-webpack-plugin

* CSS压缩：optimize-css-assets-webpack-plugin 同时使用cssnano

* HTML压缩：html-webpack-plugin，设置压缩参数

