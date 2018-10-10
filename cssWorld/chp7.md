## css世界读书笔记

@(读书笔记)

### 第七章：CSS世界的层叠规则
**涉及内容**：z-index，层叠上下文，
【博客】[深入理解CSS中的层叠上下文和层叠顺序](http://www.zhangxinxu.com/wordpress/?p=5115)


**适用场合：**
z-index属性只有和**定位元素**在一起时才会起作用，可以是正数或负数。
【tip】flex也可以设置z-index但本书不讨论

#### 层叠上下文
层叠上下文（stacking context）和层叠水平（stacking level）见博客。
另外，层叠水平不要和z-index混一起。

这三者的关系：
层叠上下文和层叠水平是概念。层叠顺序是规则。

#### 元素的层叠顺序（stacking order）
(从最优先到最后)
1. （装饰）正z-index，位置在最下面，特指层叠上下文元素的边框和背景色
2. z-index:auto或看成z-index:0，层叠水平一样
3. （内容）inline水平盒子，指的是包括inline/inline-block/inline-table元素的层叠顺序，都是同等级别
4. （布局）float浮动盒子
5. （布局）block块状水平盒子
6. 负z-index
7. 层叠上下文background/border

![enter image description here](https://image.zhangxinxu.com/image/blog/201601/2016-01-09_211116.png)

#### 层叠准则
1. 谁大谁上：层叠水平值大的在上面；
2. 后来居上：当元素的层叠水平一致、层叠顺序相同时，处于后面的元素会覆盖前面的元素。

##### 层叠上下文元素有如下特性：
- 层叠水平要比普通元素高
- 可以阻断元素的混合模式
- 可以嵌套，内部层叠上下文及其子元素均受制于外部的“层叠上下文”
- 每个层叠上下文和兄弟元素独立，当进行层叠变化或渲染时，只需要考虑后代元素
- 当发生层叠时，整个元素被认为是在父层叠上下文的层叠顺序中


##### 层叠上下文的创建
 层叠上下文是由一些特定的css创建的。
1. **根层叠上下文**：页面根元素html。

2. **定位元素和传统层叠上下文**
对于position值为relative/absolute以及Firefox/IE浏览器下含有position:fixed声明的定位元素，当其z-index值不是auto时，会创建层叠上下文。
当z-index:auto时，遵循黄金准则的第一条谁大谁上；当z-index为具体数值时，按照父级的大小进行层叠排列；当z-index为具体数值，且父级层叠水平/顺序一致，遵循后来居上。

3. **CSS3层叠上下文**
1. 元素为flex布局元素（父元素display:flex|inline-flex），同时z-index值不是auto。
2. 元素的opacity值不是1。
3. 元素的transform值不是none。
4. 元素的filter值不是none。

【eg】层叠上下文和层叠顺序导致的opacity穿透问题。
[代码](https://demo.cssworld.cn/7/5-2.php)
原因：元素的opacity值不是1就具有层叠上下文。层叠顺序是```z-index:auto```，和没有z-index值的absolute绝对定位元素是同等级的。
本例中，文字在图片前，所以当css3动画导致opacity不是1时，位于dom后面的图片就会遵循后来居上的原则盖掉文字。
【解决】
【法1】调整dom先后顺序
【法2】提高文字层叠顺序，设置z-index:1

#### ```z-index```负值
z-index支持负值。表现和层叠上下文和层叠顺序密切相关。（见上图最后2层）
作用：
 1. **可访问性隐藏。**z-index负值可以隐藏元素，只需要层叠上下文内的某一个父元素加背景色即可。优势如下：
    * 它与clip隐藏相比，元素无须绝对定位，设置position:relative也可以隐藏；
    * 它对原来的布局以及元素的行为没有任何影响，而clip隐藏会导致控件focus的焦点发生细微的变化。
 2. **IE8下的多背景模拟。**（利用before和after最多可以模拟出3层）

```
.box {
     background-image: url("1.jpg");
     position: relative;
     z-index: 0;
}

.box:before,
.box:after {
     content: '';
     position: absolute;
     z-index: -1;
}

.box:before {
     background-image: url("2.jpg");
}

.box:after {
     background-image: url("3.jpg");
}
```
3. **定位在元素的后面。**
[使用负值做出的胶带粘贴效果](https://demo.cssworld.cn/7/6-2.php)

#### 不犯二准则：
z-index最大值不要超过2。
避免了一个比一个设置的大，最后样式混乱


##### 层级计数器：
对于突然出现的高级元素或者动态浮层组件。使用js。遍历所有子元素最大z-index值，和默认z-index做比较。如果超出，则显示的组件z-index+1。没有超出就设置为9。

在此基础上，如果遇到需要js控制的浮层组件，9已经是一个足够安全的值了。
