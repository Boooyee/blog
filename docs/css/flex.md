### flex

#### 容器属性

* flex-direction

* flex-wrap

* flex-flow

* justify-content: 主轴对齐方式

  + flex-start：左对齐

  + flex-end：右对齐

  + center：居中

  + space-between：两端对齐，项目之间间隔相等

  + space-around：每个项目两侧的间隔相等。项目之间的间隔比项目与边框的间隔大一倍

* align-items：交叉轴如何对齐

  + flex-start

  + flex-end

  + center

  + baseline：项目的第一行文字的基线对齐

  + stretch（默认值）：如果项目未设置高度或者设为auto，将占满整个容器的高度

* align-content

#### 项目的属性

* order：定义项目的排列顺序

* flex-grow：定义项目的放大比例，默认为0，存在剩余空间，也不放大

* flex-shrink：定义了项目的缩小比例，默认值为1，如果空间不足，项目将缩小

* flex-basis：在分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。默认值为auto，项目的本来大小。

* flex：flex-grow、flex-shrink、flex-basis的简写，默认值为0 1 auto，后两个属性可选。

* align-self：允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性。
