## css世界读书笔记

@(读书笔记)

### 基本信息
![封面](https://img1.doubanio.com/view/subject/l/public/s29651678.jpg)
- [书目介绍](https://book.douban.com/subject/27615777/) 
- [书本配套demo](http://demo.cssworld.cn/)
- [作者博客网站](https://www.zhangxinxu.com/)
- [在线阅读](https://whjin.github.io/full-stack-development/posts/CSS%E4%B8%96%E7%95%8C/%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.html)

### 目录列表（点击可快速跳转）


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