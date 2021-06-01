### icon

#### 图标能否与文本同行，放在一个段落中

#### 实现 icon 标签原理是什么？

- 使用图片
  - 会有 http 请求
  - 放大会变虚，有锯齿
  - 不能修改颜色
- 使用 Sprit（连续图片集）

- css 绘制

- 使用矢量字体，矢量字体分为三类

  - Adobe 主导的 Type1
  - Apple Microsoft 主导的 TrueType
  - Adobe Apple Microsoft 主导的开源字体 OpenType

  - 小程序推荐 ttf、woff (woff2 不兼容低版本 iOS)

- SVG 矢量图片

#### icon 显示空白

- 可能是兼容性引起的，推荐使用 TTF 和 WOFF 格式的字体

### progress 组件

#### 如何实现一个下载文件并显示动态进度条功能？

#### progress 已产生的进度条，如何设置圆角？

#### 已经加载完进度条如何重新加载

```
this.setDate({percentValue: 0 })
if(wx.canIUse('nextTick')){
    wx.nextTick(() => {
        this.setDate({percentValue: 100})
    })
} else {
    setTimeout(() => {
        this.setDate({percentValue: 100})
    }, 17)
}

onTapReloadBtn(e){
    this.setDate({ percentValue: 0})
    this.setDate({ percentValue: 100})
}
```

#### 如何实现一个环形进度条？

- 用 canvas 绘制
```
const ctx2 = wx.createCanvasContext(canvasId,this)
const query = wx.createSelectQuery().in(this)
```
### rich-text 组件

#### 如何预览保存图片

- wx.previewImage

#### 如何解决图片之间的间隙问题

#### 如何插入广告标签/如何将 HTML 文本解析呈现

- 使用 parse 组件
