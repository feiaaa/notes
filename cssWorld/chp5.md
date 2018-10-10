## css世界读书笔记

@(读书笔记)

### 第五章：内联元素与流
**涉及内容**：baseline，line-height，vertical-align，支柱（strut）


#### 5.1 术语介绍【必读】
- **基线：baseline**，字母x的下边缘（线）就是基线。 （这个在后期会经常提到！）
- **字母x与CSS中的x-height**：x-height指的是字母x的高度。
- **vertical-align:middle**;的middle指的是基线网上1/2 x-height高度。

- **字母x与CSS中的ex**
> ex是CSS中的一个相对单位，指的是小写字母x的高度，即x-height。
> **ex的作用是不受字体和字号影响的内联元素的垂直居中对齐效果。**
> 内联元素默认是基线对齐，而基线就是x的底部，而1ex就是一个x的高度。

（关系图）
![各个术语的关系](https://upload.wikimedia.org/wikipedia/commons/thumb/3/39/Typography_Line_Terms.svg/410px-Typography_Line_Terms.svg.png)

#### 5.2内联元素的基石```line-hight```。
- ```<div>```的高度是由行高(```line-hight```)决定的，而非文字。
- 对于非替换元素的纯内联元素，其可视高度完全由```line-height```决定。和```padding，border```的属性，没有任何影响。

> **行距**:列和列之间，明显的间隙叫做行距。
> **半行距**:在css中，行距分散在当前文字的上方和下方，即使是第一行文字也是如此分布。
> 因为这个间隙的高度仅占完整行距高度的一半，因此被称为半行距。
> 【注意】区别行距和行高

三者的关系：
> 半行距*2=行距=```line-height```(行高)-```font-size```(字号)
> 所以说行距是可以为**负值**的，效果就是两行文字叠在一起。

**内容区域**，可以近似理解为firefox向文本选中带背景色的区域
【面试题】使用css3让选中的文字换色

>【复习】3.4.2 内联盒模型
> 内联盒子:
> ```<em>aa</em>```
> 行框盒子 （line box）：每一行就是一个行框盒子
> ```123456<em>aa</em>```
> 幽灵空白节点（支柱）：存在于每个行框盒子前面，同时具有该元素的字体和行高属性，宽度为0的内联盒。

```line-height```不可以影响替换元素，比如图片的高度。
之所以会把图片占据高度变高，是因为支柱（strut）的高度变高了。

图片为内联元素，会构成一个“行框盒子”，而在HTML5下，每一个“行框盒子”的前面都是一个宽度为0的“幽灵空白节点”，其内联特性表现和普通字符完全一样。

对于块级元素，```line-height```对其本身没有任何作用，改变```line-height```，块级元素的高度跟着变化实际上是通过改变块级元素里面内联元素占据的高度实现。

【个人总结】
- 纯内联元素，元素的高度，由```line-height```决定。
- 替换元素，混合内联元素，块级元素，高度和```line-height```无关，之所以会有变化，是因为**支柱**的关系。

##### 为什么```line-height```可以让内联元素“垂直居中”
>【eg】行高实现**多行文本**或图片等替换元素的垂直居中效果
> [行高与多行文字垂直居中实例页面](https://demo.cssworld.cn/5/2-4.php)
> 【原理】
>  多行文字用一个标签包裹，该标签设置```display:inline-block```。（目的是产生行框盒子，获得支柱）
> 内联元素是基线对齐，所以在该标签设置```vertical-align:middle```,辅助调整多行文字。

> 关于**单行文本**的垂直居中：
只要设置```line-height```就行了，```height```不写也没事。

##### 深入```line-height```的各类属性值
默认值:```normal```;这个值根据浏览器，中英文，字体来变化。（是一个变量）

【应用】在实际开发中，需要对```line-height```的默认值进行重置：
1. 数值。计算方法line-height=1.5*font-size
2. 百分比值。line-height=150%*font-size
3. 长度值。line-height=1.5*font-size;

因为继承上的差异，虽然都想表达1.5倍的意思，最终会有不同的结果。解决办法是：使用通配符
```
line-height:150%
```
【计算方式】行高是切掉小数点后数字，成为整数。计算务必注意。

【应用】推荐的行高
* 制作一个图文内容比较多的网站，使用数值作为单位，line-height值可以设置在1.6~1.8。
* 偏重布局结构的网站，使用长度值或数值都可以，line-height值可以设置在20px。

##### 内联元素```line-height```的“大值特性”
定义：无论内联元素```line-height```如何设置，最终父级元素的高度都是由数值大的那个```line-height```决定。
每个内联盒子外层都有一个看不见的行框盒子，在每个行框盒子前面都有一个宽度为0的具有该元素的字体和行高属性的看不见的支柱。（这个应用很丰富）

##### ```line-height```的好基友```vertical-align```
在内联元素下必定同时出现。
凡是```line-height```起作用的地方```vertical-align```也一定起作用。
后面会有范例详细讲这个。

##### ```vertical-align```的各类属性值
**默认值**：```baseline``` （基线对齐），
> 因为基线的定义是字母x的下边缘。所以内联元素默认都是沿着字母x的下边缘对齐。
> 因为中文和部分英文的下边缘低于x的边缘，所以给人感觉文字偏下，会进行调整。

其他属性值的分类还有文本类，上下标类，数值百分比类。

```vertical-align```的好处
- 兼容好。（不用写hack）
- 可以控制内联元素的垂直对齐位置。
【注意】
```vertical-align:middle```并非真正意义的垂直居中，理由如上。如果需要精确对齐，用数值。

【复习+归纳】
- ```margin/padding```是相对于宽度计算
- ```line-height```是相对于``font-size```计算
- ```vertical-align```属性的百分比值是相对于```line-height```计算。
- (但是一般来说，```line-height```相对固定，已知，所以直接用数值更方便，百分比鸡肋了。)

##### ```vertical-align```的作用前提
- 只能应用于内联元素以及```display:table-cell```的元素。也就是只能作用在display计算值为```inline、inline-block、inline-table、inline-cell```的元素上。
- 块级元素不支持。浮动元素和绝对定位会让元素块状化(见第6章)，从而导致```vertical-align```不起作用。

**【问答】为何```display:table-cell```可以无视```line-height```（```table-cell```是块级）？**
> [table-cell与vertical-align属性关系示意实例页面](https://demo.cssworld.cn/5/3-4.php)
> ```vertical-align```起作用的是```table-cell```元素自身。如果分开写在2个div里无效。
> 作用在自身后，即使子元素为块元素也没事。

```line-height```和```vertical-align```的关系，间隙产生的原因和解决办法

文字默认全部都是基线对齐，所以当字号大小不一样的两个文字在一起的时候，就会发生彼此上下的位移，如果位移距离足够大的话，就会超过行高的限制，导致出现意料之外的高度。

> 产生间隙的三大元凶：支柱，```line-height```（基线对齐），```vertical-align```。
> 解决办法
1. 图片块状化。（上面三个全干掉）
2. 容器line-height足够小。比如容器设置line-height:0;
3. 容器font-size足够小。（改变支柱的基线位置）
4. 图片设置其他```vertical-align```属性值。间隙产生的原因之一就是基线对齐，所以设置```vertical-align```的值为```top/bottom/middle```其中一个。

效果展示
[图片底部留有间隙的问题实例页面](https://demo.cssworld.cn/5/3-5.php)

##### 深入理解vertical-align线性类属性值
1. inline-block与baseline
一个inline-block元素，如果里面没有内联元素，或者overflow:visible，则该元素的基线就是其margin底边缘；
否则其基线就是元素里面最后一行内联元素的基线。
[代码演示：基线对齐问题](https://codepen.io/feiaaa/pen/bxYVmN）

【应用】
[一套基于20px图标的对齐处理技巧](https://demo.cssworld.cn/5/3-7.php)
【注意】3个要点
- 图标高度和当前行高都是20px； 
- 图标标签里永远有字符；例如``` <p><i class="icon icon-delete">删除</i>随便什么文字</p>```
- 图标css不使用```overflow:hidden```保证基线为字符里面的基线，但要让里面的潜在字符不可见```content:'\3000'```

2. ```vertical-align:top/bottom```
- 内联元素：元素底部和当前行框盒子的顶部对齐。
-``` table-cell``元素：元素底padding边缘和表格行的顶部对齐。

3. ```vertical-align:middle```与近似垂直居中
- 内联元素：元素的垂直中心点和行框盒子相对于外面的表格行居中对齐。
- ```table-cell```元素：单元格填充盒子相对于外面的表格行居中对齐。

##### 深入理解vertical-align文本类属性值
- ```vertical-align:text-top```：盒子的顶部和父级内容区域的顶部对齐。
- ```vertical-align:text-bottom```：盒子的底部和父级内容区域的底部对齐。
【注意】鸡肋功能，没什么人用

#### 【实例】于vertical-align属性的水平垂直居中弹框
[基于vertical-align的纯CSS定位弹框实例页面](https://demo.cssworld.cn/5/3-10.php)
优点：
- 节省了js代码（传统页面用js计算位置）
- 渲染速度快过js
- 可以灵活控制垂直居中的比列（也可以是在1/3处）
- 容器设置```overflow:auto```，可以实现弹框高度超过一屏幕时人能看见屏外内容。
【原理】
关键点：容器的伪元素和弹框都用了```vertical-align:middle```。
1. font-size设置为0，x中心点位置是.container的上边缘，高度100%宽度0的伪元素和这个中心点对齐。CSS中默认是左上方排列对齐，所以这个伪元素和原本在容器上边缘的x中心点一起往下移动了半个容器高度，就是此时x中心点在 容器的垂直中心线上。
2. 弹框元素.dialog设置```vertical-align:middle;```。根据定义，弹框的垂直中心位置和x中心点位置对齐，此时x中心点在容器的垂直中心位置，.dialog元素和容器垂直中心位置对齐，从而实现垂直居中效果。
3. 水平居中```text-align:center```就行
