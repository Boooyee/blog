### 滑动窗口类

用于解决在一段连续的区间中寻找满足条件的问题，比如说给定一段连续的字符串，寻找出无重复字符的最长子串。该思路主要应用于数组及字符串的数据结构中。

滑动窗口主要思路是维护一对指针，在一定条件内右移右指针扩大窗口大小直到窗口内的解不满足题意，此时我们需要根据情况移动左指针，重复移动左右指针的操作并在区间内求解，直到双指针不能再移动。



### 寻找出无重复的最长子串

```
let left = 0
let right = 0

while(right < size){
  获取当前索引数据
  right++
  数据更新等操作
  while(窗口需要缩小){
    left++
    数据移除等操作
  }
}
```


