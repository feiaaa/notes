## css世界读书笔记

@(读书笔记)

### 第九章：元素的装饰和美化
**涉及内容**：color，background。
#### 颜色关键字
css2一共17个颜色关键字（比如orange），css3共100多个。
所有css3新增的颜色关键字在**原生ie8**的浏览器上是不支持的（只支持那17个）。

##### ```transparent```
 ```background-color:transparent```从IE6开始支持。
 ```color:transparent```从IE9开始支持。

##### ```currentColor```
变量可以使用color计算值。从IE9开始支持。

##### rgba颜色
color属性不支持hsl颜色、rgba颜色和hsla颜色。

#### ```background```
多数background的特性是css3里出现的。

##### 隐藏元素的```background-image```到底加不加载
看场合。
本体设置ie下加载。firefox不加载。
webkit下，如果设置了```background-image```+隐藏，加载。如果背景图父元素被设置为```display:none``` 不加载；

IE8浏览器支持base64图片，包括在```background-image```属性中使用，可以节约网络请求。
base64图片的渲染性能不高，大尺寸图片慎用。
[使用svg来解决大图片base64代码超长的问题](https://www.zhangxinxu.com/wordpress/?p=7957)

##### 与众不同的```background-position```百分比计算方式
如果缺省偏移关键字，则会认为是center.
【博客】应用和实际看这篇[CSS background-position用法](https://www.cnblogs.com/zgqys1980/p/4308434.html)
【eg】```background-position:right 40px bottom 20px```表示距离右边缘40px，距离下边缘20px。
【公式】```position```百分比值计算公式：

- ```positionX```=（容器宽度 - 图片宽度）```precentX```
- ```positionY```=（容器高度 - 图片高度）```precentY```
- （容器宽度 - 图片宽度）×（```-50%```）的结果是一个正值。
- （容器高度 - 图片高度）×（```-50%```）的结果是一个正值。

##### ```background-attachment:fixed```
用来表示 背景相对于当前文档的视区定位。页面再怎么滚动背景也纹丝不动。
 缺点：只能局限在窗体背景图使用上，高度不能超过一屏。

##### ```background-color```背景色永远是最低的
无论background单背景还是多背景，背景色是在最底下的位置。