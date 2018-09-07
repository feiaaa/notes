## css世界读书笔记

@(读书笔记)

### 基本信息
![封面](https://img1.doubanio.com/view/subject/l/public/s29651678.jpg)
- [书目介绍](https://book.douban.com/subject/27615777/) 
- [书本配套demo](http://demo.cssworld.cn/)
- [作者博客网站](https://www.zhangxinxu.com/)
- [在线阅读](https://whjin.github.io/full-stack-development/posts/CSS%E4%B8%96%E7%95%8C/%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.html)

### 备注
- 本书只讨论css2，css3在另一本书里面
- 鉴于作者为了便于大众理解，打了很多比喻。本笔记为按照章节记录的脱水版。
- 保留了对应术语的外文单词，如有需要自行搜索。
- 要搜索，请 ```ctrl+f```搜索【面试题】，【问答】等

### 第一章节：流
**css和svg的差异** ：svg在图像上有优势，但在文字上没有。
**流：css引导元素排列和定位的一条看不见的水流；**
**流体布局：** 利用‘流’实现各类自适应效果（**1.流体布局，2.自适应性**）(具备自适应性质但非自适应布局)，（table 也有自适应性质，但非流体布局）；
css3:是为了实现更丰富和复杂的网页，和流关系不大。

### 第二章节：术语
【eg】
```
.vacabulary{
height:99px;color:transparent
}
```
- 属性：css的中文称谓```height```

- 值:大多和数字有关，又有字符串值和位置值等类型。（css3还有别的）```99px``` ```#999```
- 属性值:冒号后面所有的内容。属性值=值+关键字+功能符 ```1px solid rgba(0,0,0,0.5)```
- 变量：css2少见。```currentColor```【css3】

```
.icon {
display: inline-block;
width: 16px; height: 20px;
background-image: url(../201307/sprite_icons.png);
background-color: currentColor; /* 该颜色控制图标的颜色 */
}
```
- 关键字：特指英文单词 ```transparent```
- 长度单位:px，em（其他还有时间单位和角度单位）
>%不是长度单位 
>【eg】2%是一个完整的值 .这样一个特例，应该被叫做百分比值（见第五章5.2.3）
>【公式】<number>（数字）+长度单位=<length> （值）
>
>长度单位=绝对长度单位（px,pt等）+相对长度单位（相对字体长度单位(em，rem)+相对视区长度单位（vh,vw））

- 功能符：值以函数的形式指定（被括号括起来的那种）```rgba(0,0,0,0.5)``` ```url('地址')```  

- 声明:属性名+属性值 ```color：transparent```
- 声明块：花括号括起来的声明 ```{height:99px;color:transparent }```
- 选择器：瞄准目标元素的东西```.vacabulary ``` 包括（类选择器，id选择器，属性选择器，伪类选择器，微元素选择器）
- 关系选择器：根据与其他元素的关系选择元素的选择器。包括（后代，相邻后代，兄弟，相邻兄弟）
- 规则和规则集:选择器+声明块
```
.vacabulary{
height:99px;color:transparent
}
```
- @规则：以@字符开始的一些规则 ```@media```

> **未定义行为** ：规范没有对某种场景的具体描述（表现为浏览器之间的显示差异）。

### 第三章：流，元素与基本尺寸
**涉及内容**：块级元素，内联元素；width/height,max/min
- [w3c的display属性](http://www.w3school.com.cn/cssref/pr_class_display.asp)
#### 3.1 块级元素
> **块级元素不代表的display:block。**
>【eg】li的```display:list-item;```table的```display:table```
>【w3c解释】list-item此元素会作为列表显示。table此元素会作为块级表格来显示（类似 ```<table>```），表格前后带有换行符。
> 
> 块级元素具有换行特性，理论上能用clear清除浮动

 **【问答】为什么实际开发不用```display:list-item```**
- 出现不需要的项目符号；
- ie不支持伪类元素的display:item ```p:after{display:list-item}```这样的就不行。

**【问答】为什么list-item的元素会出现项目符号？**
- 标记盒子（marker box）：专门存放项目符号（圆点，方块）。

**作者理解的盒子构成（不是指盒模型）**：(无考证)
- 一个盒子由外在盒子和内在盒子（容器盒子，即为flow，流）组成。
- 外在盒子负责是一行流还是换行显示。内在负责高度，宽度，内容。

【eg】
```
display:inline-block
```
外在盒子是inline，内在是block。所以是一行流（能图文并排显示），又能设置宽度高度

【eg】
```
display:inline-table
```
外在inline，内在table 。所以一行流+表格显示。
注意，写法是这样的，并不是把```<table></table>```给套上去
```
<div class="inline-table">
<p>第1列</p>
<p>第2列</p>
</div>
```

**高度和宽度属性作用在容器盒子的上面。**

#### 3.2 width/height 具体作用细节

> **width:auto的4种宽度表现。**

> - **充分利用可用空间**（例如一些div元素的默认宽度是100%），
> -  **收缩到合适（shrink to fit）**，包裹性（由内容撑开）
> -  **收缩到最小**（table里面，当每一列空间都不够的时候，文字能断就断，这个时候中文可以随便断，英文单词不能断，所以每一列的宽度根据单词和中英文决定了），
> - **超出容器限制**（一般不会主动超过父容器宽度，除非设置不换行```white-space:no-wrap```）。

> 
> **外部尺寸和内部尺寸：**
>外部尺寸（extrinsic sizing）的尺寸由外部决定。内部尺寸（intrinsic sizing）的尺寸由内部元素决定。（所以上面4种表现，1是外部尺寸（跟着父元素走），剩下的是内部尺寸（跟着自己走））

【w3c解释】table-layout表格布局算法
默认值：auto （自动表格布局）没有定义宽度的格子，根据自己内容和大小来设置宽度。
其他的值：fixed （默认表格布局）根据设置好的宽度来布局，哪怕内容有超出。

> **外部尺寸和流体特性**

> - **正常流宽度的情况下，块元素不要设置宽度。**（会失去流动性，无法像水流那样完全利用容器的空间，正确的处理办法是使用盒模型，来自动分配空间）
> - **格式化宽度仅出现在绝对定位模型中。**```position:absolute fixed``` 一般来说，**绝对定位元素的宽度是由内部尺寸决定的。**在特例（非替换元素）的情况下，宽度是由外部尺寸决定的。（宽度由最近的，position非static的祖先元素决定）


### 鑫三无准则：无宽度，无图片，无浮动。
- 无宽度：外部尺寸的块级元素一旦设置了宽度，流动性就丢失了。（如果内容改变，导致内部尺寸变化，外部依然定死，非常难看）[博客地址]( http://www.zhangxinxu.com/wordpress/?p=1152)
- 无图片：尽量减少图片使用。加载快，可用css3做出效果替代[博客地址]( http://www.zhangxinxu.com/wordpress/?p=1652)
- 无浮动：

> **内部尺寸和流体特性。**
> 如何判断这个元素使用的是内部尺寸：**假如这个元素里面没有内容，宽度就是0。**
> 特性：
> - **包裹性**：包裹性=包裹+自适应性（即元素尺寸由内部元素决定，但永远小于包含块的容器的尺寸）
【eg】```display:inline-block```  内容再多，只要正常情况，宽度不会超过容器

> - **首选最小宽度**：元素最适合的最小宽度（中文为单个汉字，英文为单词，替换元素（如img） 为元素内容本身宽度）
> - **最大宽度**：元素可以有的最大宽度。实际等同于包裹性元素设置```white-space：nowrap```申明后的宽度。

**【问答】button和input[type=button]都为按钮，有什么区别？**
- css上，两个都是inline-block元素。input的white-space为pre（空格被浏览器保留），button的white-space为normal（默认，空格被忽略）。所以：**文字多的情况下，button会自动换行，input不会**
- 其他：
input是自闭合标签。button是一对闭合标签
所以input按钮内容只能放文字，button可以放图片；（<button></button>中间可以放任内容 ）
- 【提示】如果在 HTML 表单中使用 button 元素，不同的浏览器会提交不同的按钮值。请使用 input 元素在 HTML 表单中创建按钮。

**内在盒子的盒模型**：content，padding，border，margin

#### 宽度分离原则：
 width不与影响宽度的padding，border（有时算上margin ）共存。
【错误的范例】
```
.box{width:200px;padding:20px}
```
【解决办法】
分离，width独占一层标签，剩下的3个用流动性在内部自适应实现。
【正确的范例】
```
.father{width:180px}
.son{margin:0 20px;padding:40px;border:1px solid black}
```
```
<div class="father">
<div class="son"></div>
</div>
```
【原因】
盒模型的4个值都能改变元素宽度，最终宽度容易变化并导致意想不到的布局发生。
（例如突然加需求改样式，尺寸要重新计算了）。使用分离后。子元素的width是auto，自动填满父容器。在外框尺寸不变的情况下，省去计算。
【优化】box-sizing（兼容ie8+），flex（ie10+）

#### box-sizing（面试题）：盒尺寸的作用细节，为了解决宽度自适应问题
> 把标准盒模型改成怪异盒模型，保持外容器的宽度不变。
> 不支持margin，因为本来margin在两个模型下，都是不算在宽度里的。
> 不要全部元素都设置box-sizing：产生没必要的消耗；不能解决所有问题（有margin存在）；

#### height:auto
**css默认流是水平方向的，宽度是稀缺的， 高度是无限的。**所以宽度规则多。高度相对随意

#### height: 100%；
- width属性，就算父元素auto，子元素百分比没问题。
- height：父元素auto，子元素百分比无效（因为上面那句宽度有限高度无限），效果等价于auto
> 【解决办法】
> - 法1：父级必须要有一个可以生效的高度值
> - 法2：父元素100%+绝对定位
```
html,body{
height:100% /**只写body也不行，因为body也没有具体高度值*/
}
```
**浏览器渲染顺序**：渲染的时候，先渲染父，再渲染子。

#### 3.3 min-width , min-height,max-width, max-height
【应用】
只用min和max不用width：一定区间内的自适应布局方案：小屏幕的笔记本和大屏幕的电脑都能适用；
max-width：防止图片过大，在移动端影响体验。如果原始图片没有设置height，则必须max-width和height：auto配合适用。否则图片被水平压缩【缺点】加载过程中，图片高度从0变计算高度，看起来有明显的瀑布式下落

> 默认值 
> - width/height和min-width/min-height :auto;
> - max-width/max-height：none

> 优先级超越!important原因
> - max-width会盖掉width，哪怕width用了style和!important；
> - min-width要是大过了max-width,以min-width为准，max-width的值被忽略

####【应用】max-width：展开收起的动画技术
- 【法0】display:block和none之间切换
- 【法1】jquery的slideUp()和slideDown()
- 【法2】max-height:0到max-height:值的变化。（值大于展开内容高度即可，如果值太大，会造成‘效果延迟’的视觉效果）

### 3.4 内联元素
**特征：可以和文字一行显示。**
#####  内联盒模型的分类如下
- **内容区域（content area）**：本质是一个字符盒子（character box）。对于非文字类的替换元素，内容区域为元素本身。
- **内联盒子（inline box）**：让内容排成一行。其实这里的内联盒子是指元素的外在盒子。，用来决定元素是内联还是块级。子类有内联盒子和匿名内联盒子。
【eg】
```
<p>123456<em>aa</em></p>
```
内联盒子:
```
<em>aa</em>
```
匿名内联盒子:(因为后面的em是内联元素所以是匿名内联盒子。如果前/后面是块级，则是匿名块级盒子)
```
123456
```
- **行框盒子 （line box）**：每一行就是一个行框盒子
```
123456<em>aa</em>
```
- **包含盒子 /包含块（containing block）**：行框盒子+两头标签
```
<p>123456<em>aa</em></p>
```

> 连续内联盒子：一堆内联级别元素串联，中间无任何换行标签或者块级元素。
#### 支柱（strut，幽灵空白节点 ）
存在于每个行框盒子前面，同时具有该元素的字体和行高属性，宽度为0的内联盒。

### 第四章：盒尺寸四大家族
**涉及内容**：盒(一般默认为块元素)的四个属性。

#### 4.1 content
【tips】此处的content，是指```.div{content:''}```，不是```<div>content</div>```

#####  元素的分类方式
- **按块级/内联分**，分成内联元素和块级元素；
- **按可否替换分**，分成替换元素和非替换元素。

> **替换元素的定义**：通过修改某个属性值呈现的内容，就能被替换的元素，叫替换元素。
包含：```<img>```，```<object>```，```<video>```，```<iframe>```，```<textarea>```，```<input>```
非替换元素：(X)HTML 的大多数元素是不可替换元素，**他们将内容直接告诉浏览器，将其显示出来**。
比如```<p>p的内容</p>```、```<label>label的内容</label>```；
从观点上来看，与非替换元素，**最大的区别在于src属性和content属性**。
> 
> 替换元素的3个特性：
- 内容外观不受页面上css的影响， 如无法改变单选复选的样式
- 有自己的默认尺寸， video，iframe，canvas为300px*150px，img为0，表单元素根据浏览器本身
- 在很多css属性上有自己的一套表现规则。 替换元素的基线为元素下边缘，非替换元素是字符x的下边缘（详见第五章）
> 
##### 尺寸计算规则：
- 固有尺寸：替换内容原本的尺寸（默认尺寸）。css2无法改变替换元素内容的固有尺寸（css3部分可以例如```object-fit```,```background-size```）
```
<canvas id="" width="" height="" style="background: blue;"></canvas>
```
- html尺寸: 只能通过 HTML 原生属性改变，包括```<img>```的width和height属性、```<input>```的size属性，```<textarea>```的cols和rows属性等
```
<canvas id="" width="400" height="300" style="background: red;"></canvas>
```
- css尺寸:可以通过 CSS 的width和height或者max-width/min-width和max-height/min-height设置的尺寸，对应盒尺寸中的content box。
```
.canvasSize{width: 500px;height: 100px;background: yellow;}
<canvas id="" width="" height="" class="canvasSize"></canvas>
```
![尺寸关系](https://github.com/feiaaa/notes/blob/master/peitu/tihuanSize.png)

> **在优先级上，css尺寸>html尺寸>固有尺寸。**

【实例-优化】
web开发的时候，为了提高性能，一般首屏一下的图片通过滚动的方式异步加载。然而因为是异步，所以会在加载过程中出现瀑布式下落（图片从无到有高度），影响体验。
通过css控制，可以实现加载网络图片时，未加载完成的时候显示本地一张占位图，加载完成后显示网络图片；
【法1】透明图片占位
```
<img src='transparent.jpg'/> //其中src也可以不写，因为当src缺省时不会有任何请求
```
【法2】visibility （第11章节）
```
.img{visibility:hidden}
.img[src]{visibility:visible}
```

##### content属性的3条特质
- 使用content生成的文本是无法选中和复制的(虽然防止了拷贝页面但也无法被seo获取)
- 不能左右```:empty```类 [:empty](http://www.w3school.com.cn/cssref/selector_empty.asp)给空标签设定样式
- content的动态生成值无法获取。（计数器）

#####  ```:before/:after```伪元素技术的称呼和由来:
实际项目中，content内容生成技术几乎都被用在```:before/:after```这两个元素里了.（ie8仅仅支持:，不支持::）

#####  **8大用途**：
- 辅助实现两端对齐/垂直居中/上下边缘对齐。
【范例】[底部对齐的柱状图](http://demo.cssworld.cn/4/1-7.php)
【注意】第一行```<div id="box" class="box"><i class="bar"></i>```不能写成
```
<div id="box" class="box">
<i class="bar"></i>
```

- 如果写了,```text-align:justify```失效，就不是左右贴着两端对齐了
- 关于```text-align:justify```在第八章8.6.6 或者[作者博客](http://www.zhangxinxu.com/wordpress/?p=1514)

> ```:before```用于辅助实现底部对齐 （见第五章5.3.8综合实例第4点）
> ```:after```用于辅助实现两端对齐 （见第八章8.6）

- 项目列表的图标（图片图标）

```
div:before{
content:url(xxx.jpg)
}
```
也可以
```
div:before{
content:'';url(xxx.jpg)
}
```

- 项目列表中的图标（font-face图标）：
[大漠的font-face教程](https://www.w3cplus.com/css3/web-icon-with-font-face)
两种列表图标的对比：
 - 图标比较少的时候/base64体积小的时候用图片图标划算。
 - 要成体系，随时修改用font-face 。可以让设计用iconfont生成。


- loading加载效果：
操作方式：content内用转义的换行符```'\A'```,```'\D'```
【范例】[content换行符与打点loading效果实例页面](http://demo.cssworld.cn/4/1-9.php)

- 引用符号quote：
但实际上这个功能很鸡肋
```
q:before{content:'“'}
q:after{content:'”'}
```


- attr属性值：
【eg】image使用alt属性改变图片描述信息
```
img::after{
content:attr(alt)
}
```
【eg】自定义一个叫做data-title的属性
```
img::after{
content:attr(data-title)
}
```
【eg】关于自定义属性的错误示范（不能有引号）
```
img::after{
content:attr('data-title')
}
```

- 【重点】**计数器**。
【范例】[CSS计数器counter-reset值为2实例页面](http://demo.cssworld.cn/4/1-11.php)

 [ countent计数器](https://blog.csdn.net/qq_22182279/article/details/80251555)
【知识点】
两个属性（``` counter-reset```,```  counter-increment```）和一个方法（``` counter()```/```   counters()```）

 - counter-reset:设置某个选择器出现次数的计数器的值，默认值是0，chrome下可以取小数，任何小数向下取整(把小数点后面的截掉)；多个以空格分隔，而不是逗号
 - counter-increment：计数器递增的值，默认为1。
使用content:counter(...)会默认先执行一次```counter-increment```+```counter-reset```，所以content默认情况下显示的就是1。
也可以递减，```counter-increment:counter-1```
 -  counter():显示计数,支持级联
 - counters():显示计数，用于嵌套计数。（例如文章的章节列表）

【计数器的重点】**【普照规则】计数的计算方式**
普照规则：一个容器里的普照源```counter-reset```唯一，每普照```counter-increment```一次，普照源增加一次计数值。（不用在意关系，**只用在意被普照了几次**）
【eg】[CSS计数器counter-increment递增机制演示实例页面](http://demo.cssworld.cn/4/1-15.php)
【说明】：
before和after的content各显示一个计数器，所以展示出来有2个。
before这边有一个递增。基于初始值，2+1=3;
after这边再来个递增，基于3,3+1=4；

如果序号出错，考虑一下，是不是把计数显示和计数重置以兄弟的关系放一起了。
【范例】
[正确的范例](http://demo.cssworld.cn/4/1-18.php)
[错误的范例](http://demo.cssworld.cn/4/1-19.php)
可以拿对比工具对一下，发现出错的原因，是把嵌套关系写成了兄弟关系，所以出错。

- 混合特性
content各种语法可以混在一起写。
```
q:before{
content :url(xxx.png) open-quote;
}
```
【用途】在伪元素中同时显示文字和图片

#### 4.2 padding
padding在垂直方向上，同样会影响布局。虽然垂直方向上面的行为完全是```line-height```和```vertical-align```的影响。
用途：
- [增大点击区域](http://demo.cssworld.cn/4/2-1.php)
- [管道符与超链接平齐](http://demo.cssworld.cn/4/2-1.php) 
代码中的```a+a:before```看这里 [CSS 相邻兄弟选择器](http://www.w3school.com.cn/css/css_selector_adjacent_sibling.asp)

##### ```padding```的横向纵向百分比，按照宽度来作为标准。
用途：根据比例做出等比的矩形和正方形。
【应用】[首屏头图适配](http://demo.cssworld.cn/4/2-3.php)

与内联元素上的差异:断行，宽高细节差异。原因：内联元素的垂直padding会让支柱（strut）出现。

##### 其他细节：
- 有序列表和无序列表的``padding-left``单位是px。
- - 很多表单元素（```input```,``` textarea```,``` button```）都内置```padding```；
 - 所有浏览器的单选和复选框无内置padding；
  - button的padding最难控制
- 在图像上面的用途，可以绘制打开菜单的三道杠和双层圆点。
[代码](http://demo.cssworld.cn/4/2-4.php)


#### 4.3 margin
##### 4.3.1【必读】定义阐述
- 元素尺寸：对应jQuery中的```$().width/$().height()```方法，包括```padding/border```，也就是元素的```border box```的尺寸。在原生的DOM API中写成```offsetWidth/offsetHeight```，也称为元素偏移尺寸。
- 元素内部尺寸：对应jQuery中的```$().innerWidth()/$().innerHeight()```方法，表示元素的内部尺寸，包括```padding```但不包括border，也就是元素的padding box的尺寸。在原生的DOM API中写成```clientWidth/clientHeight```，也称为元素可视尺寸。
- 元素外部尺寸：对应jQuery中的```$().outerWidth(true)/$().outerHeight(true)```方法，表示元素的外部尺寸，包括padding/border/margin，也就是元素的```margin box```的尺寸。没有对应的原生的DOM API。
- 【个人总结】元素尺寸-```border```;元素内部尺寸-```padding```;元素外部尺寸-```margin```

##### ```margin```与元素的内部尺寸（```padding```）
>【复习】3.2.1```width:auto```的4种宽度表现。
 - 充分利用可用空间（例如一些div元素的默认宽度是100%），
 - 收缩到合适（shrink to fit），包裹性（由内容撑开）
 - 收缩到最小
 - 超出容器限制

> 内部尺寸和流体特性。特性：
- 包裹性：包裹性=包裹+自适应性（即元素尺寸由内部元素决定，但永远小于包含块的容器的尺寸）
-  首选最小宽度
- 最大宽度

**【结论】**
- 对于padding，元素设定width或者保持‘包裹性’的时候，会改变可视元素的尺寸；
- 对于margin，元素设定width或者保持‘包裹性’的时候，对元素的尺寸无影响；只有元素是‘充分利用可用空间’时，才会改变可视元素的尺寸；

>【面试题】负边距表现出来是怎样的。
> 尺寸变大。此时元素是‘充分利用可用空间’。

【eg】
```
<div class="father">    
	<div class="son"></div>
</div>

.father { width: 300px; }.son { margin: 0 -20px; }
```
> 此时，.son元素的宽度是340px（300+20+20）

【应用】通过负边距做出的自适应布局。
【eg】【书上例子】左侧定宽右侧自适应
```
.box { overflow: hidden; }
.box > img { float: left; }
.box >p { margin-left: 140px; }
<div class="box">
    <img src="xxx.jpg">
    <p>文字内容……</p>
</div>
```
> 左侧图片定死，右侧p文字自适应，此处并未用到负边距;

【eg】【书上例子】右侧定宽左侧自适应，且dom的书写无需颠倒位置。（需要颠倒的原因见第六章float）
```
.box { overflow: hidden; }
.full { float: left; width: 100%; }
.box > img { float: left; margin-left: -128px; }
.full>p { margin-right: 140px; }

<div class="box">
    <div class="full">    
        <p>文字内容……</p>
    </div>
    <img src="1.jpg">
</div>

```
> 左侧full文字自适应，右侧图片定死。
> 左侧full宽度占满，但P元素留下了右边margin；
> 右侧img在float以后，靠负边距撑开，满足‘充分利用可用空间’。
> 注意负边距要大于需要的定宽宽度，否则会掉下去。

【eg】【书上例子】两端对齐效果
【法1】flex。
【法2】使用CSS3的nth-of-type() 选择器原理：利用父容器
![div排列两端对齐](https://img2.mukewang.com/5786038b0001fd3612800720.jpg)

##### ```margin```与元素的外部尺寸。
对于普通的块状元素，在默认的水平流下，margin只能改变左右方向的内部尺寸，垂直方向上无法改变。
如果我们是用writing-mode的，改变流向为垂直流，则水平方向内部尺寸无法改变，垂直方向可以改变，这是由margin:auto的计算规则决定的。

【eg】【书上例子】【浏览器间差异】有滚动条的情况下，借助外部尺寸特性来实现底部留白。
【带bug的代码 】
```
<div style="height: 100px;background: yellow;overflow:  scroll;padding: 50px 0;">
           <img src="img/HBuilder.png" height="114" style=""/>
</div>
```
> 此处表现为：父级高度小于图片高度，带滚动条，设定了上下50px的内边距。但是在chrome下正常，在ie和firefox下，下边距的50px没了。
> 原因：未定义行为（见第2章术语）。chrome触发滚条的条件是子元素超过content box。ie和firefox的是子元素超过padding box。
【修复后的代码】
```
<div style="height: 100px;background: yellow;overflow: scroll;">
        <img src="img/xxx.png" height="114"  style="margin: 50px 0;"/>
</div>
```
> 【注意】
> 必须触发滚条；margin留白写在子元素内。


【eg】【书上例子】【面试题】等高布局
[代码](http://demo.cssworld.cn/4/3-2.php)
关键点：
```
margin-bottom: -9999px;
padding-bottom: 9999px;
```

> 【原理】
> 垂直方向```margin```上无法改变元素的内部尺寸，但是能够改变外部尺寸。因为```margin-bottom: -9999px;```意味着元素的外部尺寸在垂直方向上变小。
> 默认情况下，垂直方向块级元素的上下距离是0，一旦```margin-bottom: -9999px;```，就意味着后面所有的元素和上面元素的空间距离变成了-9999px，也就是后面元素都往上移动了。
> ```padding```增加高度元素，正负抵消，对布局成无影响，却给我们需要的是视觉层多了9999的高度的可使用的背景色。
> 因为数值太大，所以配合父级```overflow:hidden```，把多出来的色块藏掉，形成了视觉上的等高布局效果。

**等高布局的其他方法和比较**
- 【margin】：
优点：兼容ok，支持多个分栏。缺点：触发锚点定位会有奇怪的定位问题。子元素在父元素外，使用overflow:hidden成了限制。
- 【border】：
优点：兼容ok，无锚点定位问题。缺点：最多三栏，不支持百分比（因为border不支持，这个在4.4讲）；只能实现至少一侧定宽布局（范例见4.4.6）
- 【table-cell】优点：天然等高，缺点：只有IE8以上浏览器才坚持。

- 【总评】如果项目无需兼容ie6,7，则推荐使用table-cell实现等高布局。

#### 4.3.2 ```margin```的百分比（一个没什么应用价值的功能）
- ```margin```在垂直方向上无法改变元素自身内容尺寸。需要父元素做载体。
- ```margin```的百分比值无论是水平方向还是垂直方向都是相对于宽度计算的
容易出现边距合并问题，有时要设定双倍尺寸。

#### 4.3.3 margin合并
**定义**：块级元素的上外边距和下外边距，有时候会合并成单个外边距。
**2个条件**，块级元素，只发生在垂直方向（当前文档流方向相垂直）上。

**3种场景**：相邻元素的margin合并，父级和第一个/最后一个子元素合并，空块级元素合并。

> 避免合并的办法：
> 针对第2个场景：
- 父元素变成块级格式化上下文 (Block Fromatting Context,简称[BFC](https://www.cnblogs.com/libin-1/p/7098468.html)，见6.3章)
- 父元素设置```border-top/padding-top```/```border-bottom/padding-bottom```
- 父元素和第一个/最后一个子元素间添加内联元素分隔

> 【eg】
> margin合并导致头图掉下来可以添加```.container{overlfow:hidden;}```进行修复。
> 其原理是通过设置overflow属性让父级元素块状格式化上下文。

> 针对第3个场景：
- 设置垂直方向的border/padding；
- 设置height/min-height;
- 里面添加内联元素（直接空格不行）

【面试题】**计算规则**：正正取大值，正负相加，负负最负值

**【问答】```*{margin:0;padding:0}```和边距合并**
> 应该看场合使用。绝大多数网站可以这么干，如果是博客，文章类网站，应该统一标签的margin大小而不是重置为0。
> 任何地方放```<div>```，都不会影响布局，但是```<div>```是没有语义的。不代表任何特定类型的内容，只用来分组，分隔。
> - 父子合并的意义：不合并可能会导致间距很大，违背了```div```设计的作用，影响阅读体验。
> - 自身合并的意义：可以避免不小心遗落或生成空标签，影响排版布局。

#### 4.3.4  ```margin:auto```
- 元素就算没有设置宽度或者高度，也会自动填充。
```
 <div></div>
```
> 宽度自动填满。

- 元素没有设置高度或者宽度，也会自动填充对应的方位。
```
<div></div>

div{
position:absolute;
left:0;
right:0;
}
```
> 宽度还是自动填满。

> **填充规则**
- 一侧定值，一侧auto，auto占满剩余空间的大小。
- 两侧均为auto则平分剩余空间。

【eg】让某个块状元素右对齐。
【法1】float。
【法2】margin-left：auto
```
<div style="width: 300px;background: yellow;">
          <div style="width: 100px;background:  green;margin-left: auto;">
                        111
          </div>
</div>
```
【原理】margin的初始值0，如果说缺了一边，另一边有auto，则正好是偏向缺省这边的对齐效果。

【面试题】用该特性做水平垂直居中。
【法1】top50%再margin-height/2
【法2】绝对定位+宽高定死+```margin:auto```，缺点：ie8+支持
[代码](http://demo.cssworld.cn/4/3-5.php)


【复习】第三章 外部尺寸和流体特性 的第二条 ：   
格式化宽度仅出现在绝对定位模型中。```position:absolute fixed``` 一般来说，绝对定位元素的宽度是由内部尺寸决定的。在特例（非替换元素）的情况下，宽度是由外部尺寸决定的。（宽度由最近的，position非static的祖先元素决定）
【原理】
在子元素只写了绝对定位时，尺寸表现为 “格式化宽度和格式化高度”，即为外部尺寸，可以自动填满父容器。
宽高被定死以后，原来该被填满的空间空了出来，用auto平分剩余空间

##### margin无效的原因

- display计算值inline的非替换元素的垂直margin是无效的。
对于内联替换元素，垂直margin有效，并没有margin合并问题。所以图片永远不会发生margin合并。

- 表格中的```tr```和```td```元素，或者设置```display:table-cell,display:table-row```元素的margin是无效的。设置```display:table-caption,display:inline-table```元素的ok

- margin合并的时候更改margin值是没有效果的。
- 绝对定位元素，非定位方向的margin值无效。
```
img{top:30%,margin-right:10px} //margin-right写了，right没写，无效
```

- 定高容器的子元素的margin-bottom或者，宽度定死的子元素的margin-right定位失效。

- 鞭长莫及，导致margin无效。（需要理解float和overflow ，第六章）
- 内联特性导致margin无效。

#### 4.4 border
```border-width```不支持百分比的两个原因：
- 如果内容变大，边框也按比例变大，不符合语义化；
- 容易造成没对齐的假象

**重置：**```border:0```或者```border:none```

##### 冷门的border-style
【面试题】双线边框。```border-style:double```

##### border-color的跟着color走
【eg】应用图标变色。
[代码](http://demo.cssworld.cn/4/4-1.php)

##### 透明边框的三大技巧：
- 右下方background的定位技巧
- 增加点击区域大小
- 三角形绘制。

**【eg】border等高布局**
[代码](http://demo.cssworld.cn/4/4-4.php)
【注意】
等高布局的优劣对比见4.3.1
最多实现2-3栏，等比例的最多7栏。父容器不能用```overflow:hidden```

【个人】综合练习

### 第五章：内联元素与流
**涉及内容**：baseline，line-height，vertical-align，支柱（strut）


#### 5.1 术语介绍【必读】
- **基线：baseline，**字母x的下边缘（线）就是基线。 （这个在后期会经常提到！）
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
[table-cell与vertical-align属性关系示意实例页面](https://demo.cssworld.cn/5/3-4.php)
```vertical-align```起作用的是```table-cell```元素自身。如果分开写在2个div里无效。
作用在自身后，即使子元素为块元素也没事。

```line-height```和```vertical-align```的关系，间隙产生的原因和解决办法

文字默认全部都是基线对齐，所以当字号大小不一样的两个文字在一起的时候，就会发生彼此上下的位移，如果位移距离足够大的话，就会超过行高的限制，导致出现意料之外的高度。
产生间隙的三大元凶：支柱，```line-height```（基线对齐），```vertical-align```。
解决办法
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