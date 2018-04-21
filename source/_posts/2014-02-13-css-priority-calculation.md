---
layout: post
title: 'CSS优先级计算'
date: 2014-02-13 08:11
comments: true
categories: 前端
---
我们知道，当css样式同时作用于某个元素时，会产生层叠。浏览器层叠各个来源样式的顺序为：

1.浏览器默认样式表

2.用户样式表

3.作者链接样式表（按照它们链接到页面的先后顺序）

4.作者嵌入样式

5.作者行内样式

浏览器会按照上述顺序依次检查每个来源的样式，并在有定义的情况下，更新对每个标签属性值的设定。整个检查更新过程结束后，再将每个标签以最终设定的样式显示出来。

浏览器层叠还有一套层叠机制来计算优先级。

* !important

如果某个声明后面加上`!important`则表明此样式具有最高权重。如果有多个声明使用`!important`则按照下面的规则来计算优先级。

* 计算特指度(Specificity)

我们使用`style属性-I-C-E`来表示特指度。这里`style属性`表示行内样式。比如：

`<p style="color: #ffffff"></p>`

`I` 表示id，`C`表示class，`E`表示element。

下面这个图很好的诠释了CSS特指度：（来自http://css-tricks.com/specifics-on-css-specificity/）

![](http://liunian.info/img/images/2010/08/specificity-calculationbase.png)

1.某个元素使用行内样式，则在`style属性`的位置上加1；

2.选择符中有一个ID，就在I的位置上加1；

3.选择符中有一个类，就在C的位置上加1；

4.选择符中有一个元素（标签）名，就在E的位置上加1；

我们通过一些例子来说明：（来自http://css-tricks.com/specifics-on-css-specificity/）

![](http://css-tricks.com/wp-content/csstricks-uploads/cssspecificity-calc-1.png)
![](http://css-tricks.com/wp-content/csstricks-uploads/cssspecificity-calc-2.png)
![](http://cdn.css-tricks.com/wp-content/uploads/2010/05/cssspecificity-calc-3v2.jpg)
![](http://css-tricks.com/wp-content/csstricks-uploads/cssspecificity-calc-4.png)
![](http://css-tricks.com/wp-content/csstricks-uploads/cssspecificity-calc-5.png)

注意：

1.(*) 选择符没有特指度 (0,0,0,0)

2.伪元素 (e.g. ::first-line) 在`E`上加1，而不是在`C`上加1

3.伪类选择器:not()本身不加特指度，只是在括号里面的元素加上特指度

举个例子来说明：

我们创建这样一个html文档：

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf8">
<title>CSS特指度计算</title>
<link rel=stylesheet href="style.css" />
<style type="text/css">
em {color: blue;}
</style>
</head>

<body>
<article id="first">
<p class="odd"><em style="color: black">第一段文字</em></p>
<p>第二段文字</p>
</article>
</body>
</html>
```

style.css内容如下：

```css
article#first p.odd em {
    color: red;
}
```

这里为`<em>`设定了3个样式：分别是行内样式，嵌入样式和外部样式表。因为行内样式的特指度最高，所以应用行内样式，结果显示为`<em>`标签下的文字颜色为黑色。

<p class="sassmeister" data-gist-id="10694291" data-height="480"><a href="http://sassmeister.com/gist/10694291">Play with this gist on SassMeister.</a></p><script src="http://static.sassmeister.com/js/embed.js" async></script>

如果去掉行内样式：

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf8">
<title>CSS特指度计算</title>
<link rel=stylesheet href="style.css" />
<style type="text/css">
em {color: blue;}
</style>
</head>

<body>
<article id="first">
<p class="odd"><em>第一段文字</em></p>
<p>第二段文字</p>
</article>
</body>
</html>
```

则`<em>`标签下的文字颜色为红色，因为外部样式表中特指度比嵌入样式高。

![](http://ww3.sinaimg.cn/large/90b90757gw1ea1x3f4dyaj209w058a9w.jpg)

如果在嵌入样式上加上`!important`则文字显示为蓝色。

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf8">
<title>CSS特指度计算</title>
<link rel=stylesheet href="style.css" />
<style type="text/css">
em {color: blue !important;}
</style>
</head>

<body>
<article id="first">
<p class="odd"><em>第一段文字</em></p>
<p>第二段文字</p>
</article>
</body>
</html>
```

<p class="sassmeister" data-gist-id="10694435" data-height="480"><a href="http://sassmeister.com/gist/10694435">Play with this gist on SassMeister.</a></p><script src="http://static.sassmeister.com/js/embed.js" async></script>

* 设定的样式胜过继承的样式

设定的样式胜过继承的样式，此时不用考虑特指度（即显式设定优先）。下面简单解释一下，比如下面的标记

```html
<div id="cascade_demo">
   <p id="inheritance_fact">Inheritance is <em>weak</em> in the Cascade</p>
</div>
```

和下面的规则

```css
div#cascade_demo p#inheritance_fact {color:blue;}
2 - 0 - 2 （高特指度）
```

会导致单词“weak”变成蓝色，因为它从父元素p那里继承了这个颜色值。

但是，只要我们再给em添加一条规则

```css
em {color:red;}
0 - 0 - 1 （低特指度）
```

em就会变成红色。因为，虽然它的特指度低（0-0-1），但em继承的颜色值，会被为它明确（显式）指定的颜色值覆盖，就算（隐式）遗传该颜色值的规则的特指度高（2-0-2）也没有用。

**参考**：

1.http://css-tricks.com/specifics-on-css-specificity/

2.《CSS设计指南》
