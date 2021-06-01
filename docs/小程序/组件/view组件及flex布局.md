### view 容器组件

- hover-class：指定按下去样式类

- hover-stop-propagation：

- hover-start-time：按住多久出现点击态

- hover-stay-time：手指松开后点击态保留时间

微信小程序优势：没有点击延迟

### flex 布局

- justify-content:主轴

  - flex-start
  - flex-end
  - center
  - space-between
  - space-around

- align-items：
  - stretch
  - flex-atsrt
  - flex-end
  - center
  - baseline：以子元素第一行文字对齐，设置文本居中显示
- flex-wrap

- align-content：

- flex-direction

#### 如何把 view 上的内容绘制在画布上，生成一张海报

- view 目前不能直接生成，可以使用 painter 组件完成

### 可移动容器及可移动区域，实现侧滑删除 `movable-view` `movable-area`

### `scroll-view` 实现滚动锚定

### `scroll-view`渲染一个滚动长列表

- refresher-enabled
- refresher-threshold

### 使用选择器组件

- 自定义实现省市区三级联动

### 基于 wxs 实现一个竖向的 slider

### 页面链接组件：自定义一个导航栏

### image 实现图片懒加载

### 实现直播间功能

- live-pusher/live-player 组件主要属性及使用限制
- 开启使用腾讯云的云直播功能
- 安装使用 ffmepg，使用 ffmepg 进行推拉流验证
- 使用 live-pusher/live-player
- 同层渲染
- live-pusher/live-player 组件开发中常见问题

### webview

webview 是一个基于 webkit 的引擎，可以解析 DOM 元素，展示 html 页面元素的控件。他和浏览器展示页面原理是相同的，所以可以把它当作浏览器看待。

### 导航组件

- function-page-navigator(插件中使用)

- navigator
