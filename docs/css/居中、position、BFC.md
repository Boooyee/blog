### css动画属性

Transform：2D转换方法，允许旋转，缩放，倾斜或者平移给定元素。只能转换由盒模型定位的元素，元素具有display:block; 由盒模型定位。

* rorate：顺时针旋转的角度
* scale：设置元素方法或缩小的倍数
* translate：沿着X和Y轴移动元素

### 垂直居中实现

* 使用绝对定位和负外边距对块级元素进行垂直居中（需要知道被居中元素尺寸）

* 使用绝对定位和transform

``` 
#child{
  background:orange;
  position:absolute;
  top:50%;
  transform: translate(0,-50%);
}
```

这种方法好处是不需要知道被居中元素尺寸，因为transform中translate偏移量百分比就相当于元素自身尺寸。

* flex布局

* line-height对单行文本进行垂直居中

### position属性

* static：position默认值
* relative/absolute/fixed 相同点相对于某个基点的定位，不同点是基点不同。

 - relative：相对于默认位置（static）进行偏移，定位点是元素的默认位置。搭配top、bottom、left、right使用。

 - absolute表示相对于上级元素进行偏移，定位基点是父元素。有个重要限制是定位基点不能是static定位，否则会变成基于整个网页根元素html定位。absolute会被正常文档流忽略。
 - fixed：基于视口进行偏移。
 - sticky：是relative和fixed的结合。必须搭配top、bottom、left、right四个属性一起使用，不能省略，否则等同于releative定位，不产生动态固定的效果。具体规则是：当页面滚动，父元素开始脱离视口时（部分不可见），只要与sticky元素距离达到生效门槛，relative定位自动切换为fixed定位。等到元素完全脱离视口，fexed定位自动切换为relative定位。

### BFC

* BFC（块级格式化上下文）：具备BFC特性的元素，就像是被一个元素包裹，容器内的元素在布局上不会影响外面的元素。

* 常见应用：
  + 解决普通文档流外边距折叠问题
  + 清除浮动
  + 阻止普通文档流被浮动元素覆盖

* 如何触发BFC
  + 根元素
  + 浮动元素（float不为none的元素）
  + 绝对定位元素（position为absolute和fixed）
  + 行内块元素（display:inline-block）
  + overflow不为visible的元素

### 移动端1px解决

产生原因：设备像素比DPR（默认缩放100%）时，设备像素和CSS像素比值。主流屏幕DPR=2，也就是说要实现物理像素1px，css只能设置0.5。

解决方案：

* 使用边框图片
* 使用伪元素

``` 
.setOnePx{
  position: relative;
  &::after{
    position: absolute;
    content: '';
    background-color: #e5e5e5;
    display: block;
    width: 100%;
    height: 1px; /*no*/
    transform: scale(1, 0.5);
    top: 0;
    left: 0;
  }
}
```

* box-shadow设置

* viewport + rem方案：通过设置缩放，让css像素等于真正的物理像素。

``` 
const scale = 1 / window.devicePixelRatio;
    const viewport = document.querySelector('meta[name="viewport"]');
    if (!viewport) {
        viewport = document.createElement('meta');
        viewport.setAttribute('name', 'viewport');
        window.document.head.appendChild(viewport);
    }
    viewport.setAttribute('content', 'width=device-width,user-scalable=no,initial-scale=' + scale + ',maximum-scale=' + scale + ',minimum-scale=' + scale);
```
