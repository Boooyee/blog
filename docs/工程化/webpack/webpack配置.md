### 可维护的webpack构建配置

构建配置抽离成npm包的意义：
 + 通用性
   + 业务开发者无需关注构建配置
   + 统一团队构建脚本
 + 可维护性
   + 构建配置合理的拆分
   + README文档、ChangeLog文档
 + 质量
   + 冒烟测试

构建配置包设计：
  + 基础配置：webpack.base.js
    + 资源解析
      + 解析ES6
      + 解析react
      + 解析css
      + 解析less
      + 解析图片
      + 解析字体
    + 样式增强
      + css前缀补齐
      + css px转换成rem
    + 目录清理
    + css提取成单独页面
    
  + 开发环境：webpack.dev.js
    + 代码热更新（css热更新，js热更新）
    + sourcemap

  + 生产环境：webpack.prod.js
    + 代码压缩
    + 文件指纹
    + Tree shaking
    + Scope hoisting
    + 速度优化
    + 体积优化
  + SSR环境：webpack.ssr.js

抽离成npm包统一管理

通过webpack-merge组合配置：module.exports = merge(baseConfig,devConfig);

