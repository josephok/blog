---
layout: post
title: 'css position'
date: 2014-02-13 08:10
comments: true
categories: 前端
---
CSS position属性用来设定元素的位置。

语法：

position: static | relative | absolute | sticky | fixed

* static

默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。

* relative

生成相对定位的元素，相对于其正常位置进行定位。

举个例子来说：

html代码部分：

```html
<section id="one">1
</section>
<section id="two">2
</section>
<section id="three">3
</section>
<section id="four">4
</section>
```

CSS代码部分：

```css
#one, #two, #three, #four {
  text-align: center;
  font-size: 36px;
  color: white;
  display: inline-block;
  margin: 10px;
  width: 60px;
  height: 60px;
  background-color: red;
}

#two {
  position: relative;
  top: 10px;
  left: -10px;
}
```

显示结果：

<p class="sassmeister" data-gist-id="10694501" data-height="480"><a href="http://sassmeister.com/gist/10694501">Play with this gist on SassMeister.</a></p><script src="http://static.sassmeister.com/js/embed.js" async></script>

上例中`#two`向下移动了10px，向左移动了10px。

* absolute

生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。如果父元素没有设定position或者position设定为`static`则继续向上找父元素直到找到根元素。

元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。

举个例子：

HTML代码：

```html
<section id="parent">
<section id="child">
</section>
</section>
```

CSS代码：

```css
#parent {
  width: 200px;
  height: 200px;
  background-color: black;
}
#parent #child {
  width: 100px;
  height: 100px;
  background-color: red;
  position: absolute;
  top: 0;
  right: 0;
}
```

显示结果为：

![](http://ww2.sinaimg.cn/large/90b90757gw1ea10ibi7b3j218e09lweo.jpg)

上例中`#child`元素设定position为`absolute`，但是其父元素的position没有设置（默认为`static`），所以`#child`会相对于body定位：  top: 0;  right: 0; 即显示在页面的右上角。

如果把`#parent`元素的position设定为`relative`或者`absolute`或者`fixed`，则显示结果为：

<p class="sassmeister" data-gist-id="10694599" data-height="480"><a href="http://sassmeister.com/gist/10694599">Play with this gist on SassMeister.</a></p><script src="http://static.sassmeister.com/js/embed.js" async></script>

* fixed

生成绝对定位的元素，相对于浏览器窗口进行定位。
元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。
元素不会受浏览器滚动条的影响。

[查看例子](http://www.w3schools.com/css/tryit.asp?filename=trycss_position_fixed)
