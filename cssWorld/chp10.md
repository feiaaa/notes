## css世界读书笔记

@(读书笔记)

### 第十章：元素的显示与隐藏
**涉及内容**：display:none;visibility:hidden。
####  使用CSS让元素不可见的方法，剪裁、定位到屏幕外、明度变化等。
![让元素不可见的方法](https://github.com/feiaaa/notes/blob/master/peitu/hide.png)
（见表）
#### ```display```与元素的显示/隐藏
 ```display:none``` 该元素和所有后代元素都隐藏，如果其他计算值则显示。
 ```display:none``` 的显隐控制并不会影响css3 的animation动画实现，但是会影响transition过渡效果实现。
所以transition和visibility组合密切。
对于计数器，使用了 ```display:none```，则该计数不算到总值里去。

#### ```visibility```与元素的显示/隐藏
![让元素不可见的方法](https://github.com/feiaaa/notes/blob/master/peitu/hide.png)
（见表）
【eg】[visibility后代可见](https://demo.cssworld.cn/10/2-1.php)
[display和visibility的计数器页面](https://demo.cssworld.cn/10/2-2.php)
[visibility与内容hover的延时显示实例页面](https://demo.cssworld.cn/10/2-3.php)

###第十一章：用户界面样式
**涉及内容**：outline,cursor。
#### ```outline```
outline表示元素的轮廓，语法和border属性类似，分宽度、类型、颜色，支持的关键字和属性值和border属性一模一样。

【注意】绝不可以在全局设置 ```outline:0 none```
会导致input的focus高亮标记失效，导致填表的时候，不知道键盘聚焦到哪里了，提交的时候受影响。

#### 真正不占据空间的outline及其应用
1. 案例一：头像剪裁的矩形镂空效果
2. 案例二：自动填满屏幕剩余空间（可以是底部空间）的应用技巧

#### ```cursor```
[光标大全和演示效果](http://www.w3school.com.cn/tiy/t.asp?f=csse_cursor)

#### 自定义光标
【eg】透明光标：例如看视频的时候不想被打扰
【eg】相册的向左向右光标：方便切换

### 第十二章：流向的改变
**涉及内容**：```direction```，```writing-mode```

#### 改变水平流向的```direction```
默认值：
```
direction:ltr //默认值，left to right
```
【用途】
【eg】取消和提交按钮左右互换位置却不改变布局。
【eg】在文字前面打点 [代码](https://demo.cssworld.cn/12/1-2.php) 
【eg】改变表格的排列顺序
【eg】对于两端对齐的元素，最后一行右对齐显示 [代码](https://demo.cssworld.cn/12/1-3.php)

##### ```unicode-bidi```
双向性，改变文字的阅读方向。例如：英文从左往右读，阿拉伯文从右往左读。混写的时候，需要用这个
[代码](https://demo.cssworld.cn/12/1-4.php)

#### 改变纵横规则的```writing-mode```
将页面的默认水平流改成了垂直流。
【应用】古诗词（本来的用途是这个）
[代码](https://codepen.io/feiaaa/pen/yxRLKM)
【应用】文字的点击下沉效果
缺点：只能用在一个字上，否则会穿帮
[CSS writing-mode与text-indent文字下沉实例页面](https://demo.cssworld.cn/12/2-5.php)
【应用】做成打开下拉菜单导致图标旋转的效果
[图标旋转](https://demo.cssworld.cn/12/2-6.php)

```direction```和```writing-mode```关系：
direction改变的是垂直方向上内联元素的文本方向。
writing-mode改变的是流的方向。