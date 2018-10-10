## css世界读书笔记

@(读书笔记)

### 第八章：强大文本处理能力
涉及内容：em，rem，font-size ，文本控制，font属性。

#### ```line-height```和```font-size```的计算比例
vertical-align百分比值属性是相对于line-height计算
line-height的数值属性和百分比值都是相对于font-size计算。

##### ```font-size```和```ex em rem```
1em的计算值等同当前元素所在的font-size计算值。
rem=root em 。根元素em大小。

##### 理解font-size的关键字属性值
1. **相对尺寸关键字**。larger、smaller ，根据当前元素```font-size```计算
2. **绝对尺寸关键字**。```xx-large、x-large、large、medium、small、x-small、xx-small```

实际策略：
- 一般会对html或者body重置font-size大小。
- 容器设置font-size:medium，此时这个局部展示区域的字号跟随浏览器的设置，默认计算值是16px。
- 容器内的文字字号全部使用相对单位，如百分比值或em，然后基于16px进行转换。

#####  ```font-size:0```与文本的隐藏
中文情况下，chrome的font-size计算值不能小于12 （就算设置了小于12的，按12算）

【用途】隐藏logo对应元素内的文字，除了text-indent缩进隐藏外，还可以使用以下方法：
```
.logo {
    font-size: 0;
}
```
#### 字体属性家族font-family
 ```font-family```支持两类属性值，一类是字体名，一类是字体族。
如果字体名包含空格，需要用引号包起来，也可以多个设定逗号分隔
```
h1{
font-family:'PingFang SC','Microsoft Yahei'
}
```

##### 等宽字体的用途
- 利于代码呈现（阅读舒服）
- 模拟图像呈现案例[下拉菜单选择图案](https://demo.cssworld.cn/8/2-1.php)
- ch和等宽布局（例如11位手机号的输入，设置宽度为11ch，可以很明显看到是否漏数字）

想要使用中文字体，必须知道对应名称[对照表](http://www.xwbetter.com/font-family/)

繁体中文：微软正黑。大陆：微软雅黑。

#### 其他成员
##### ```font-weight```，粗细
400=normal，700=bold。 注意，取值为100-900之间的整百数（可以取100，200，但是不可以取150）
##### ```font-style```,倾斜样式
- ```font-style:normal```
- ```font-style:italic```，使用当前字体的斜体
- ```font-style:oblique```，单纯让文字倾斜 （和上面的区别：有些字体有自己设计的斜体，样式和倾斜的不一样）
##### ```font-variant```
对中文没用，```font-variant:small-caps```表现出英文小体型大写字母

#### ```font```属性：进行文本相关样式的缩写
完整语法：
 ```[font-style||font-variant||font-weight]?font-size[/line-height]?font-family```，（font-size和font-family是必需项）。

font缩写会破坏部分属性的继承性，必须要带上```font-family```。
对于```font-family```属性值过长的解决方法
- 使用一个系统不存在的字体名字占位，再设置```font-family:inherit```重置占位
- 利用```@font face```规则将字体列表重定义为一个字体。

##### 使用关键字值的font属性
font属性支持关键字属性值，
语法：```font:caption|icon|menu|message-box|small-caption|status-bar```
【注意】
使用关键字作为属性值必须是独立的，不能添加```font-family```或```font-size```等。
考虑chrome的市场占有率，在使用font时，避开```caption,icon,message-box```

##### 应用价值：让字体跟着系统走。
好处1：变成情感化设计，引起用户好感。（有些会自定义字体）
好处2：系统更新字体换掉，与时俱进
【eg】使用系统字体
```
html:{font-family:system-ui}
```

#### ```@font face```规则
 ```@font face```的**本质**上就是一个定义字体或字体集的**变量**，包括字体重命名、默认字体样式设置等。
【博客】[真正了解CSS3背景下的@font face规则]( https://www.zhangxinxu.com/wordpress/?p=6063)

##### 推荐使用方法：
```
@font-face {
    font-family: ICON;
    src: url("icon.eot") format('eot');
    src: local('☺'),
    url("icon.woff2") format("woff2"),
    url("icon.woff") format("woff"),
    url("icon.ttf") format("truetype");
}
```

【说明】
> 关于src下各个格式的说明
- eot格式是IE私有的。（舍弃）
- woff是专门为Web开发而设计的字体格式。（次优先使用）
- woff2是比woff尺寸更小的字体，是Web开发首选字体。（优先使用）
- ttf格式作为系统安装字体比较多。（舍弃）

> 上面那个代码为脱水版，还有一些鸡肋的属性不写也没关系；
 local()表示系统安装的字体;
 

> 【eg】响应式图标：字号较大时，图标细节更更丰富。字号小时图标简单。
[代码：借助font-weight实现响应式图标实例页面](https://demo.cssworld.cn/8/5-1.php)

#### 文本控制
##### ```text-indent```与内联元素缩进
首行缩进：如果是图文混排，掺杂英文和数字，则无法保证每个段落前每个两个中文空格的间距，反而有参差不齐的效果。
【eg】首行缩进和悬挂缩进（说明和注解见范例内）
[代码](https://codepen.io/feiaaa/pen/dqqwKG)


##### 相关的知识
1. ```text-indent```仅对第一行内联盒子内容有效；
2. 非替换元素以外的display计算值为inline的内联元素设置```text-indent```值无效，如果计算值是```inline-block/inline-table```则会生效。因此，如果父级块状元素设置了```text-indent```属性值，子```inline-block/inline-table```需要设置```text-indent:0```重置。
3. ```input```标签按钮```text-indent```值无效。
4. ```button```标签按钮```text-indent```值有效，但存在兼容性问题。
5. ```input```和```textarea```输入框的```text-indent```在低版本IE下有兼容性问题。

##### ```letter-spacing```与字符间距
 ```word-spacing```和```letter-spacing```共同**特性**：
1. 具有继承性；
2. **默认值都是normal**,不是0。
3. **都支持负值**，都可以让字符重叠，不适合使用word-spacing来清除空白间隙。
4. 都支持小数值。
5. 间隔算法都会受到text-align:jusitify两端对齐的影响。

**两者差异**：
 ```letter-spacing```作用域所有字符，```word-spacing```仅作用于空格字符（增加空格的间隙宽度）。

##### 了解```word-break```和```word-wrap```的区别
**word-break属性语法**：
- word-break:normal：
- word-break:break-all：**允许任意非CJK（Chinese/Japanese/Korean）文本间的单词断行**

**word-wrap语法**：
- word-wrap:normal：正常的换行规则
- word-wrap:break-word：**一行单词中没有换行点时换行**，CSS3中名称overflow-wrap

**区别：**
 ```word-break:break-all```的作用是所有的都换行
 ```word-wrap:break-word``则是如果这一行文字有了可换行的点（如空格或CJK等），就在这些换行点换行。
【eg】效果对比
[代码](https://demo.cssworld.cn/8/6-5.php)

#####  ```white-space```与换行和空格的控制
1. ```white-space```的处理模型
声明了如何处理元素内的空白字符，包括空格键Space、回车键Enter、制表符Tab产生的空白。
功能有3个维度，分别是：**是否合并空白字符(多个合为一个)、是否合并换行符(多行合为一行)、文本是否自动换行（文本环绕）。**
![white-space不同属性](https://github.com/feiaaa/notes/blob/master/peitu/whitespace.png)
（最后2项ie8+支持）

2. ``white-space``与最大可用宽度
**当```white-space```设置为```nowrap``时，元素的宽度表现为最大可用宽度，换行符和一些空格全部合并，文本一行显示。**
【用途】
 1. 包含块尺寸大小处理。绝对定位和```inline-block```元素都具有包裹性，当文本内容宽度超过包含块宽度时，就会发生文本环绕现象。
 2. 单行文字溢出...效果。```text-overflow:ellipsis;``文字内容超出打点效果离不开``white-space:nowrap;```声明。
 3. 水平列表切换效果。如果列表的数目是不固定的，使用```white-space:nowrap;```使列表一行显示。[代码](https://demo.cssworld.cn/8/6-6.php)

##### text-align与元素对齐
 ```text-align:justify```想要实现两端对齐布局效果，需要满足两点：
一是有分隔点，如空格；二是要**超过一行，此时不是最后一行内容会两端对齐**。
【解决】
【法1】借助伪元素补一行，塞一个块状内联元素加一行然后隐藏掉
【法2】(仅ie,仅文字)```text-align-last```属性，可以规定最后一行内联内容的排列方式。
```
.justify {
    text-align-last: justify;
}
```
【法3】(仅图文列表排版)利用第二点的原理，加一排，凑够一排需要的占位标签个数。
[代码 文字的两端对齐](https://demo.cssworld.cn/8/6-7.php)
[代码 图片的两端对齐](https://demo.cssworld.cn/8/6-8.php)

##### 如何解决```text-decoration```下划线和文本重叠的问题
对于纯内联元素，垂直方向的padding属性和border属性对原来的布局定位等没有任何影响。
【应用】【eg】使用border-bottom模拟text-decoration下划线解决文本重叠的问题
```
a{
text-decoration:none;
border-bottom:1px solid;
padding-bottom:5px;
}
```

【优点】
调节下边缘距离；兼容好；hover后，下划线也会和文字color一样变色；可以改变下划线样式（虚线等）

#### text-transform字符大小写
 ```text-transform```是为英文字符设计，全大写```text-transform:uppercase```，全小写```text-transform:lowercase```
1. 场景一：身份证输入
2. 场景二：验证码输入

#### 了解```:first-letter/:first-line```伪元素
**伪元素和伪类的划分**： 一个冒号的开头叫做伪类；两个冒号的叫做伪元素。

##### 深入```:first-letter```伪元素及其实例
1. ```::first-letter```伪元素生效的前提
- 元素的display计算值必须是```block、inline-block、list-item、table-cell/table-caption```，其他值没有用。**(flex和table无效)**
- 可以直接作为伪元素的字符是数字、英文字母、中文、$、一些运算符，以及一些“空格”。
- 字符前面不能有图片或```inline-block/inline-table```之类的元素存在。

2. ```::first-letter```伪元素可以生效的CSS属性
字符被选做```::first-letter```伪元素，只有一部分有效：
用 ```visibility:hidden;display:none```隐藏伪元素无效。

3. ```::first-letter```伪元素特点
-支持部分display属性值标签嵌套。
 ```::first-letter```伪元素获取可以跨标签，不仅能选择匿名内联盒子，还能透过层层标签进行选择。
[代码演示](https://demo.cssworld.cn/8/7-1.php)
-颜色等权重总是多了一层。
 ```::first-letter```伪元素作为子元素存在，对于color继承属性，子元素的CSS设置一定比父元素的级别要高，即使使用了```!important```，子元素会先继承，然后再应用自身设置。


4.【应用】货币单位 ：第一个货币字符大写
```
<p class="price">￥399</p>
.price:first-letter {
     margin-right: 5px;
     font-size: xx-large;
     vertical-align: -2px;
}
```
##### 深入```:first-line```伪元素可以生效的CSS属性
不支持```padding,margin,border```
只能用在块级上（和```first-letter```一致）
[代码演示](https://demo.cssworld.cn/8/7-2.php)
【注意】想用```:first-line```，首行不能混入```inline-block\inline-table```
