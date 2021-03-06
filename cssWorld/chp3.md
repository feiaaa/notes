## css世界读书笔记

@(读书笔记)

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

#### 【应用】max-width：展开收起的动画技术
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