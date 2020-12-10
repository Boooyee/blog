### css动画-无限旋转
``` 

.ig{
   -webkit-animation:circle 1.5s infinite linear;
  }
  @-webkit-keyframes circle{
      0%{ transform:rotate(0deg); }
      100%{ transform:rotate(360deg); }
  }
```


### div内显示两行或三行，超出部分省略号显示

```


```

### 用css或者js实现多行文本溢出省略效果，考虑兼容性

```
.one-line{
  overflow:hidden;
  text-overflow:ellipsis;
  word-break:break-all;
}

.more-line{
  overflow:hidden;
  text-overflow:ellipsis;
  word-break:break-all;
}
```