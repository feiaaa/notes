## css世界读书笔记

@(读书笔记)

### 第六章：流的破坏与保护
**涉及内容**：float，BFC，overflow，position


#### 6.1 流的本质，特性，作用机制 

##### 本质
- 在```float```没有出现前，复杂布局是通过```<table>```完成的。
- 本质 :```float```为了实现文字环绕效果.

##### ```float```自身特性：
 - 包裹性；（【复习】包裹性=包裹+自适应性）
 - **块状化并格式化上下文（BFC）；**
 - **破坏文档流**；（特性的精髓）
 - 没有任何margin合并。

##### 作用机制（特性讲解）
> **块状化**：元素一旦float的属性值不为none，则其display计算值为block或table。
>  ```text-align```对浮动元素无效（无法控制浮动元素的左右对齐）。

>  **高度坍塌 ** ：为了实现文字环绕效果  ,float浮动属性让父元素高度坍塌。
> 【tip】[高度坍塌的代码演示](https://codepen.io/feiaaa/pen/xaPvgz) 

> **行框盒子和浮动元素的不可重叠性**：行框盒子如果和浮动元素的垂直高度有重叠，那行框盒子在正常定位下，只会跟随浮动元素，而不会发生重叠。

> **行框盒子区域限制** ：块状盒子中的行框盒子被浮动元素限制，没有任何重叠。不改变布局方式，无法通过其他css来前边区域的大小。

**【问答】为了避免高度坍塌，我给这两需要浮动的元素设置高度？**
> 文字环绕效果是由**两个特性（父级高度坍塌和行框盒子区域限制）**共同作用的结果。
定高只能解决父级元素高度坍塌带来的影响，但是对行框盒子区域限制没有任何效果，结果导致的问题是浮动元素垂直区域
>  一旦超出高度范围，或下面元素margin-top负值上偏移，就很容易使后面的元素发生“环绕效果”。
> 【错误的范例】
> [高度限制下依然发生float环绕效果实例页面](https://demo.cssworld.cn/6/1-1.php)


##### 更深的作用机制
- 浮动锚点（float anchor）是float元素所在的流中的一个点，这个点本身并不浮动。其作用是产生行框盒子，因为浮动锚点表现如同一个空的内联元素，有内联元自然就有行框盒子。
- 浮动参考（float reference）指的是浮动元素对齐参考的实体。

 float元素的浮动参考是行框盒子，（【复习】3.4.2 每一行就是一个行框盒子）也就是float元素在当前行框盒子内定位。

##### float与流体布局
- 【eg】[利用float破坏流特性的两栏自适应布局实例页面](https://demo.cssworld.cn/6/1-2.php)
- 【eg】[小说末尾的上下章节翻页功能](https://codepen.io/whjin/pen/YjrGyL)

【注意】自适应的部分，用margin来保持间距，不要写死宽度，否则失去的流动性。（例如拖动窗口，改变显示区域的宽度的时候，因为定死宽度，直接掉到下面）

#### 6.2  ```clear```
```clear```专门用来处理```float```属性带来的高度坍塌等问题。
**官方对```clear``` 的解释：元素盒子的边不能和前面的浮动元素相邻。（即后面的元素管不到）**
所以清理浮动的时候，clear只能清理前面的，不能清理后面的。

##### ```float```的其他属性值
> ```clear: none | left | right | both```

 - ```none```：默认值，左右浮动
 - ``` left```：左侧抗浮动
 - ``` right```：右侧抗浮动
 - ```both```：两侧抗浮动

**【问答】为什么常见的只有```both```？**
1. ```float```要么左边要么右边，没有两边都选。
2. 既然选择了一边，另一边一定是clear不到的
3. 当```left```有效的时候```right```一定无效。此时等同于设置了```both```。反过来当```right```有效的时候```left```一定也无效。
4. 所以```left```和```right```并没有什么用。

##### 成事不足败事有余的clear （clear的坑）
```clear```属性只有**块级元素**才有效，而```:after```等伪元素默认都是内联水平，这就是伪元素清除浮动影响时需要设置display属性值的原因。
```clear:both```的作用本质是**让自身不与```float```元素在一行显示**，并不是真正意义上的清除浮动。
1. 如果```clear:both;```元素前面的元素就是```float```元素，则```margin-top```负值即使设成```-9999px```，也没有效果。
2. ```clear:both;```后面的元素依旧可能发生文字环绕的现象。

#### 6.3 CSS世界的结界——BFC
##### 定义：
- **块状格式化上下文（BFC）**；block formatting context；
- **内联格式化上下文（IFC）**；inline formatting context；（这个略过）

#####  特性和表现原则
**表现原则 **：具有BFC特性的元素的子元素不会受到外部元素的影响，也不会影响外部元素。（里面出不去，外面进不来）

根据原则而出现的情况：
- BFC不会触发margin重叠（因为margin重叠影响外部元素）
- 可以用来清除浮动。（子元素不清除，导致父元素高度坍塌，影响后面的布局和定位，也算影响外部元素）

##### 如何触发BFC

-  html根元素；
- overflow的值为auto、scroll或hidden；
- display的值为table-cell、table-caption和inline-block中的任何一个；
- float的值不为none；
- position的值不为relative和static。

只要元素符合上面任意一个条件，就无须使用```clear:both;```属性去清除浮动的影响。

##### BFC与流体布局
【博客】[CSS深入理解流体特性和BFC特性下多栏自适应布局]( http://www.zhangxinxu.com/wordpress/?p=4588)
**【复习 表现原则】**具有BFC特性的元素的子元素不会受到外部元素的影响，也不会影响外部元素。

>【用途】自适应布局
 (p162 流体自适应布局和bfc自适应布局 )[https://codepen.io/feiaaa/pen/xaYabd]

> 基于BFC特性的自适应布局有如下优点：

1. 自适应内容由于封闭更加健壮，容错性更强。内部设置clear:both;不会与float元素相互干扰而导致错位。
2. 自适应内容自动填充浮动以外区域，无需关心浮动元素宽度，可以整站大规模应用。

> 基于BFC特性的自适应布局有如下缺点（导致有优势却没有大规模应用的原因）

 1.  ``` float:left;```。浮动元素本身BFC化，然而浮动元素具有破坏性和包裹性，失去了元素本身的流体自适应性，因此无法用来实现自动填满容器的自适应布局。
 2. ```position:absolute;```。脱离文档流，不容易操作。
 3. ```overflow:hidden;``块状元素的流体特性保存得很好，加上BFC的独立区域特性，而且从IE7开始就支持，兼容性很好。唯一的问题是容器盒子外的元素可能会被隐藏掉。
 4. ```display:inline-block;```让元素尺寸包裹收缩， 不是我们想要的block流动特性
 5. ``` display:table-cell``` 让元素表现的像单元格，ie8+支持。但会出现第三章一柱擎天的效果
 6. ```display:table-row ```无法自适应容器剩余空间
 7. ```display:table-caption```没什么用

###### IE7及以上版本浏览器适配的自适应解决方案：

 1. 借助overflow属性，如下:
```
.lbf-content {
overflow: hidden;
}
```
2.  融合```display: table-cell;```和```display: inline-block;```，如下：
```
.lbf-content {
display: table-cell;
width: 9999px;
}
```

 ```display: table-cell;```元素内连续英文字符无法换行的问题：
```
.word-break {
display: table;
width: 100%;
table-layout: fixed;
word-break: break-all;
}
```
#### 6.4 最佳结界overflow
要彻底清除浮动的影响，最适合的属性是```overflow```，和BFC组合使用清除浮动。
 ```overflow:hidden;```声明不会影响元素原先的流体特性或宽度表现。
 ```overflow```的原本属性是指定块容器元素的内容溢出时是否需要裁剪。结界是衍生效果。
visible：默认值

【面试题】写一下清除浮动的样式。

##### 了解overflow-x和overflow-y
IE8+浏览器，overflow增加了两个属性，overflow-x和overflow-y，分别表示单独控制水平或垂直方向上的剪裁规则。
支持的属性值和overflow属性一模一样：
- visible：默认值
- hidden：剪裁
- scroll：滚动条
- auto
**不会出现一个方向溢出剪裁或滚动，另一个方向内容溢出显示的效果。**

可以默认出现滚条的两个标签```<html>```,```<textarea>```
默认值都是auto。（不是visible）

浏览器与滚条
1.默认滚动条来自html，而不是body标签。
【去掉默认滚条的方式】```html {overflow: hidden;}```，移动端用js
2.滚条会占用容器的可用宽度或高度。
【解决方式】移动端的滚条一般为悬浮模式，没事。
pc端会有一个17px 的差值。
【法1】<table>最后一列自适应，前面宽度定死。
【法2】<table>定宽，右侧留17px
3.（基于2）页面变宽会导致水平方向的晃动
【解决办法】```html {overflow-y:scroll;}```

【eg】采用双table做出的表头固定，表格内容可滚动的样式
[表头固定而内容滚动的表格（滚轴在表格外）](https://blog.csdn.net/yiifaa/article/details/52104698)

[表头固定而内容滚动的表格（滚轴在表格内） ](https://codepen.io/feiaaa/pen/qMxJQW)

基于webkit 的自定义滚条

术语说明（还有其他3个不怎么用所以略过）
1. ```::-webkit-scrollbar   ```定义了滚动条整体的样式；
2.  ```::-webkit-scrollbar-thumb  ```滑块部分；
3. ```::-webkit-scrollbar-track```  轨道部分；

【eg】基于webkit 的自定义滚条
![自定义滚条](https://images2015.cnblogs.com/blog/897472/201705/897472-20170502160918836-545708149.png)
[博客和代码](https://www.cnblogs.com/lfhy/p/6796653.html)

#####【eg】单行和多行 文字溢出...效果
[代码演示](https://codepen.io/feiaaa/pen/WgMWmw)
【说明】
- 单行文本的这三条样式是最精简的了，一条都不能少。（组合起来的意思是：不换行+超出剪切+文字溢出时样式点点点）
- 多行文本的这三条,也是一条不能少。 
但是：虽然书上说无需依赖```overflow: hidden```，但不加还是会有诡异的现象（多行的时候确实是末尾...了，但是多余部分没有隐藏）；另外，只有-webkit内核有效，其他无效（IE/Firefox）
```
.ell-rows-2 {
    display: -webkit-box;//将对象作为弹性伸缩模式
    -webkit-box-orient: vertical;//检索对象的盒子的排列方式
    -webkit-line-clamp: 2;//显示的行数
}
```

##### overflow与锚点定位
###### 基于URL地址的锚链(location.hash)实现锚点跳转的方法有两种：
1. a标签以及name属性
2. 使用标签的id属性 

 **最兼容的，简洁的跳转方式**
（因为并不是所有的元素都有name 属性，具体点[这里](https://m.imooc.com/qadetail/132677)

```
<a href="#1">发展历程</a> //点击会跳的位置（相当于发送端）
<p id="1"></p>//点击会被跳到的位置（相当于接受端）
```

> 锚点定位行为的触发条件
1. URL地址中的锚链与锚点元素对应并有交互行为；
2. 可focus的锚点元素处于focus状态。

focus锚点定位指的是类似链接或按钮、输入框等可以被focus的元素在被focus时发生的页面重定位现象。
```
<input type="" name="inputs" id="inputs" value="" />
<a href='javascript:'  onclick="document.querySelector('#inputs').focus();">focus点击后的路径效果 js</a>
```

> 区别
URL地址锚点定位是让元素定位在浏览器窗体的上边缘
而focus锚点定位`是让元素在浏览器窗体范围内显示即可。

（个人）
使用URL地址锚点定位 的缺点是url后面会带有#号，并且回退的时候有记录。
focus锚点定位 ，还要搭配一点平滑动js之类。需要依赖js 。（focus也不是什么标签都能用）

###### 锚点定位作用的本质
锚点定位行为的发生，本质上是通过改变容器滚动高度或宽度来实现。本质是改变了scrollTop或scrollLeft的值。
【误区和说明】
- 锚点定位也可以发生在普通容器元素上，而且定义行为的发生时由内而外的。
由内而外：普通元素和窗体同时滚动式，会由内而外的触发所有可滚动的窗体的锚点定位行为。
- 设置了```overflow:hidden```的元素也是可以滚动的。
 ```overflow:hidden```和```overflow:auto``` ```overflow:scroll```的差别就在于有没有滚条。（如果设置了hidden，并且超高，滚条不存在，滚动依然存在）
【eg】[focus锚点定位和overflow的选项卡切换效果实例页面](https://demo.cssworld.cn/6/4-3.php)
【注意】容器要定高；由内而外会触发窗体重定位（解决办法：focus锚点定位，只要定位元素在容器里就不会发生窗体滚动）
【html代码 】
```
<div class="box">
    <div class="list"><input id="one">1</div>
    <div class="list"><input id="two">2</div>
    <div class="list"><input id="three">3</div>
    <div class="list"><input id="four">4</div></div><div class="link">
    <label class="click" for="one">1</label>
    <label class="click" for="two">2</label>
    <label class="click" for="three">3</label>
    <label class="click" for="four">4</label></div>
```

【原理】不会跳动的原因：
每个li塞一个看不到的input框。选项卡变label元素。用for属性和input的id关联
使用scrollTop解决 列表部分区域在浏览器外，仍会跳转的问题。

```
$('label.click').removeAttribute('for').on('click', function () {
    $('.box').scrollTop(xxx);//滚动数值
});
```

> 基于父容器自身的scrollTop值改变来实现自定义滚动条效果【优点】
1. 实现简单，无须做边界判断。```container.scrollTop=99999;```，列表滚动就是scrollTop值，实时获取。
2. 可与原生的scroll事件天然继承，无缝对接。
3. 无须改变子元素的结构。

#### 6.5 float的兄弟```position:absolute```
【复习 6.1 float 的特性】
##### float自身特性：
- 包裹性；（【复习】包裹性=包裹+自适应性）
- 块状化并格式化上下文（BFC）；
- 破坏文档流；（特性的精髓）
- 没有任何margin合并。

块状化：元素一旦float的属性值不为none，则其display计算值为block或table。
【复习完毕】

 ```position:absolute```的特性：包裹性，破坏性，块状化BFC，无依赖性，相对定位特性。
absolute是非常独立的css属性值。样式和行为表现不依赖其他任何css属性就能完成。

absolute和float写在一起，float没有任何效果，所以不要一起用

absolute的块状化：元素一旦position的属性值为absolute或者fixed，则其display计算值为block或table。
自适应性的最大宽度由“包含块”决定。

**包含块：containing block **
普通元素的百分比宽度是相对于父元素的content box宽度计算，而绝对定位元素的宽度是相对于第一个position不为static的祖先元素计算的。
	1. 根元素html被称为“初始包含块”，其尺寸等同于浏览器可视窗口的大小。
	2.  对于其他元素，如果该元素的position是relative或static，则“包含块”由其最近的块容器祖先盒子的content box边界组成。
	3.  如果元素position:fixed;，则“包含块”是“初始包含块”。
	4.  如果元素position:absolute;，则“包含块”由最近的position不为static的祖先元素建立，具体方式如下：
如果该祖先元素是纯inline元素，则规则略复杂：
-   给内联元素的前后各生成一个宽度为0的内联盒子，则这两个内联盒子的padding box外面的包围盒子就是内联元素的“包含块”。
-  如果该内联元素被跨行分割，则包含块是未定义的。
- 否则，“包含块”由该祖先的padding box边界组成。


> 和常规元素相比，绝对定位元素的包含块有3个明显的差异：
	1.  内联元素也可以作为包含块所在的元素；
	2. 包含块所在的元素不是父级块元素，而是最近的position不为static的祖先元素或根元素；
	3.  边界是padding box而不是content box。


内联元素的“包含块”是由“生成的”前后内联盒子决定，与里面的内联盒子细节没有任何关系。
【注意】不同浏览器下跨行兼容性问题，会导致包含块有不同的差异

**无依赖性（相对定位特性 ）**：没设置left/right/top/bottom属性的绝对定位。

【用途】
【eg】[利用absolute制作左上角new角标](https://demo.cssworld.cn/6/5-4.php)
其中，左上角，可以不写```left:0;top:0```

【eg】[导航栏各类图标定位](https://demo.cssworld.cn/6/5-5.php)
【eg】[超越常规布局的排版，用户注册页（注意提示信息）](https://demo.cssworld.cn/6/5-6.php)

【eg】[下拉列表的定位](https://demo.cssworld.cn/6/5-7.php)
【注意】虽然无依赖绝对定位好处多多，但只用在静态交互效果上较好。动态列表用js

##### absolute与text-align
absolute元素的display计算值是块状的，text-align不会起作用。
如果起了作用，是支柱和无依赖绝对定位共同作用的结果。

【eg】[text-align和absolute定位全兼容版本实例页面](https://demo.cssworld.cn/6/5-9.php)
核心代码：
```
p {
    text-align: center;
}
img {
    position: absolute;
}

```
【原理】导致图片在中间才开始显示的原因
img是内联水平。支柱也是内联水平。所以受```text-align:center```影响，水平居中显示。
img设置了absolute，表现为“无依赖绝对定位”，所以在 支柱后面显示。因为图片不占空间，所以支柱在水平中心显示。

【eg】根据上述内容做出的水平居中定位
[水平居中](https://codepen.io/feiaaa/pen/yxKGby)
【应用】[text-align实现的右外侧定位效果实例页面(右下角的返回顶部)](https://demo.cssworld.cn/6/5-10.php)

#### 6.6absolute与overflow
**overflow对absolute的裁剪规则**：如果overflow不是定位元素，同时绝对定位元素和overflow容器之间也没有定位元素，则overflow无法对absolute元素进行裁剪。overflow元素父级是定位元素也不会裁剪。

【个人】（裁剪要满足2个条件，1overflow，2本体或子元素有绝对定位）
【本书】能被裁剪的场合：
1. overflow属性所在的元素同时也是定位元素，里面的绝对定位元素会被裁剪；
2. overflow元素和绝对定位元素之间有定位元素，也会被裁剪。
3. overflow属性值不是hidden，而是auto或scroll，即使绝对定位元素高度比overflow元素高度大，也不会出现滚动条。
【应用】[个人心得]博客背景锁定
[overflow:auto与absolute滚动定位实例页面](https://demo.cssworld.cn/6/5-11.php)

#### 6.7absolute与clip
clip属性要起作用，元素必须是绝对定位absolute或固定定位fixed。

【应用】
1. fixed固定定位的剪裁
```
.fixed-clip {
    position: fixed;
    clip: rect(30px 200px 200px 20px);
}
```
2. 最佳可访问性隐藏
最佳可访问性隐藏指的是视觉上看不见，但是辅助设备(例如盲人用的)能够进行识别和访问。
同时SEO能识别，也能被focus
```
.logo h1 {
    position: absolute;
    clip: rect(0 0 0 0);
}
<a href="/" class="logo">
    <h1>我是标题</h1>
</a>
```
隐藏方面的问题详见第10章

#### absolute的流体特性
##### 当absolute遇到left/top/right/bottom属性
当absolute遇到left/top/right/bottom属性时，absolute元素才真正变成绝对定位元素。
（复习：没遇到的时候具有无依赖性）

##### absolute的流体特性
当一个绝对定位元素，其对立定位方向属性同时具有定位数值时，就会发生流体特性。
普通元素流体特性只有水平方向（默认），但是绝对定位元素可以让垂直方向和水平方向同时保持流动性。
【eg】使box完全遮盖浏览区可视窗口，并且跟着浏览器窗口大小变化。（是和窗口一样大，如果有滚条照样暴露）
```
.box{position:absolute;left: 0;right: 0;top: 0;bottom: 0;background:  rgba(0,0,0,0.5);}
<div class="box"></div>
```
【eg】利用垂直方向上的流体特性制作高度自适应布局，高度等比例布局（垂直方向上的流体让子元素可以用百分比了）
```
            <style type="text/css">
                  .box{ position: absolute;left: 0;right: 0;top:  0;bottom: 0;background: rgba(0,0,0,0.5);}
                  .box1{height: 33.3%;box-sizing: content-box;border:  1px solid red;}
            </style>
            <div class="box">
                  <div class="box1">
                        1
                  </div>
                  <div class="box1">
                        2
                  </div>
                  <div class="box1">
                        3
                  </div>
            </div>
```

##### absolute的margin:auto
绝对定位元素的margin规则和普通流体元素一致。但是ie8+支持。
【应用】利用绝对定位的流体特性和margin:auto的自动分配特性实现居中。

#### ```position:relative```
对absolute的限制：只有relative可以让元素保持在正常的文档流中。
特性：相对自身进行偏移定位，无侵入（一般情况下不会影响周围元素的布局）

relative的left/top/right/bottom百分比值是相对于包含块计算的，不是自身。
relative出现对立方向定位时，只有一个方向的定位属性能起作用，一般是左和上。

##### ```position:relative```的最小化影响原则
1. 尽量不适用relative，定位某元素，可以使用无依赖的绝对定位；
2. 如果需要使用relative，则该relative需要最小化。

#### ```position:fixed```固定定位
 ```position:fixed```固定定位元素的包含块是根元素。
唯一可以限制固定定位元素的是<html>根元素。relative对fixed定位没有任何限制作用。
无依赖的固定定位；和无依赖的绝对定位类似；没设置left/right/top/bottom属性的相对定位。

> 【应用】背景锁定
为何写背景锁定？
1蒙版层无法盖住滚条。2鼠标滚动背景依然可以滚动，影响视觉
【法1】absolute模拟fix定位。让滚条由内部普通元素产出而非窗口
【法2】用js，移动端项目阻止使用touchmove，pc端让根元素```overflow:hidden```
在加上以下js代码，通过透明border属性，把滚条的位置留出来。
[背景锁定的蒙版层](https://codepen.io/feiaaa/pen/eLLvzP)
