## css世界读书笔记

@(读书笔记)

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
