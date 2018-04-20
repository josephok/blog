---
layout: post
title: 'sass简介'
date: 2014-02-13 07:56
comments: true
categories: 
---
[Sass](http://sass-lang.com/)是一种CSS预处理器。它可以让开发者少写一些CSS代码。

* 安装Sass

Sass是用Ruby写成的，可以使用[Ruby Gems](http://rubygems.org/)安装。

`gem install sass`

安装完成后查看Sass版本：

`sass -v`

此时最新版本是
`Sass 3.2.12 (Media Mark)`

* 编译CSS

Sass文件后缀名是.scss，意为(Sassy CSS)
 
`$ sass input.scss output.css`

上面这个命令将input.scss文件编译为output.css文件。

还可以在编译时加上参数，比如--style。此参数表明编译后的css风格。

SASS提供四个编译风格的选项：

　　1.  nested：嵌套缩进的css代码，它是默认值。

　　2.  expanded：没有缩进的、扩展的css代码。

　　3.  compact：简洁格式的css代码。

　　4.  compressed：压缩后的css代码。

如：

`$ sass --style compressed input.scss output.css`

将源文档编译为压缩的css代码。

你也可以让Sass监听某个文件或目录，一旦源文件有变动，就自动生成编译后的版本。

```
# 监听单个文件：

sass --watch input.scss:output.css

# 监听整个目录：

sass --watch app/sass:public/stylesheets
```

* CSS扩展

1.嵌套规则

```
#main p {
  color: #00ff00;
  width: 97%;

  .redbox {
    background-color: #ff0000;
    color: #000000;
  }
}
```

将被编译为：

```
#main p {
  color: #00ff00;
  width: 97%; 
}
#main p .redbox {
    background-color: #ff0000;
    color: #000000; 
}
```

`.redbox`为`p`的后代元素。

```
#main {
  width: 97%;

  p, div {
    font-size: 2em;
    a { font-weight: bold; }
  }

  pre { font-size: 3em; }
}
```
将被编译为：

```
#main {
  width: 97%; 
}
#main p, #main div {
    font-size: 2em; 
}
#main p a, #main div a {
      font-weight: bold; 
}
#main pre {
    font-size: 3em; 
}
```
这样可以减少很多重复输入。

2.父元素选择符&

```
a {
  font-weight: bold;
  text-decoration: none;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
}
```
将被编译为：

```
a {
  font-weight: bold;
  text-decoration: none; 
  }
a:hover {
    text-decoration: underline; 
    }
body.firefox a {
    font-weight: normal; 
}
```
上例中&被替换为当前元素的父元素。

&可以用在深沉嵌套里面，它可以被正确地替换为当前元素的父元素。
例如：

```
#main {
  color: black;
  a {
    font-weight: bold;
    &:hover { color: red; }
  }
}
```
将被编译为：

```
#main {
  color: black;
}
#main a {
    font-weight: bold; 
}
#main a:hover {
      color: red;
}
```

这里&被替换为`a`。

3.属性嵌套

比如我们有一系列的属性如：font-family, font-style, font-weight, font-size等等，它们都有相同的前缀`font`，那么可以这样写：

```
font: {
    family: fantasy;
    style: italic;
    size: 30em;
    weight: bold;
 }
```
 
 * 注释
 
 Sass支持两种注释：/* */（多行注释） 和 //（单行注释）
 在编译时，多行注释会保留到css文件中，而单行注释则不予保留。比如：

<pre>
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output.
*/
body { color: black; }

// These comments are only one line long each.
// They won't appear in the CSS output,
// since they use the single-line comment syntax.
a { color: green; }
</pre>

编译为：
<pre>
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body {
  color: black; 
}

a {
  color: green; 
}
</pre>

* SassScript 脚本

Sass脚本支持变量，运算和函数等等。

1.变量

SassScript变量以$符号开头。

定义一个变量：

`$width: 5em; //语句以分号(;)结尾。` 


然后就可以引用它。

```
#main {
  width: $width;
}
```
2.数据类型

SassScript支持6种主要的数据类型：

* 数值（1.2， 13， 10px等）
* 文本字符串，可以加或不加引号("foo", "bar", baz等)
* 颜色（blue, #04a3f9, rgba(255, 0, 0, 0.5)等）
* 布尔值（true, false）
* nulls(null)
* 列表值，由空格或者逗号分割（1.5em 1em 0.2em， Helvetica, Arial, sans-serif）

3.运算符

SassScript支持标准的数学运算符：（+, -, *, /, %）

```
p {
  width: 1in + 8pt;
}
```
将被编译为：

```
p {
  width: 1.111in; 
}
```

除法运算符(/)比较特殊。css中有些属性可以使用(/)符号，比如`font: 10px/1.5`，那么SassScript中什么情况表示除法呢？有3中情况：

1.如果表达式含有变量

2.如果使用括号()包围

3.如果表达式作为另一个算数运算的值

如下：

```
p {
  font: 10px/8px;             // Plain CSS, no division
  $width: 1000px;
  width: $width/2;            // Uses a variable, does division
  height: (500px/2);          // Uses parentheses, does division
  margin-left: 5px + 8px/2px; // Uses +, does division
}
```
将被编译为：

```
p {
  font: 10px/8px;
  width: 500px;
  height: 250px;
  margin-left: 9px; 
}
```
如果想使用变量而不使用除法运算符，需要将变量放在#{}里面。
如：

```
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};
}
```
将被编译为：

```
p {
  font: 12px/30px; 
}
```

* 字符串操作：

`+`可以用来连接字符串。

```
p {
  cursor: e + -resize;
}
```
将被编译为：

```
p {
  cursor: e-resize; 
}
```
如果带引号的字符串`+`不带引号的字符串则结果为带引号的字符串；如果不带引号的字符串`+`带引号的字符串，结果为不带引号的字符串。总结起来就是：谁在左边听谁的！

举个例子：

```
p:before {
  content: "Foo " + Bar;
  font-family: sans- + "serif";
}
```
将被编译为：

```
p:before {
  content: "Foo Bar";
  font-family: sans-serif; 
}
```

* 差值运算符

SassScript变量还可以用于选择器和属性名，只需用#{}包围即可。如：

```
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
} //貌似代码还写多了，搞毛线？
```
将被编译为： 

```
p.foo {
  border-color: blue; 
} 
``` 
`#{}`也可以用于属性值，例如上面例子中的`font: #{$font-size}/#{$line-height}; //防止除法运算`

缺省变量`!default`

将`!default`放在变量赋值的后面表示：如果变量有值则不管，如果变量没有值则使用此条赋值语句给变量赋值。

例如：

```
$content: "First content";
$content: "Second content?" !default; //$content已经有值，此条忽略
$new_content: "First time reference" !default;
//$new_content没有赋值，则执行此条为$new_content赋值为"First time reference"

#main {
  content: $content; // "First content";
  new-content: $new_content; // "First time reference"
}
```

变量被赋值为null则相当于没有值。

```
$content: null;
$content: "Non-null content" !default;

#main {
  content: $content; // "Non-null content"
}
```

* @规则

1.@import

@import默认导入Sass文档.scss 或者 .sass，文件后缀可以省略。

`@import "foo.scss";`

或者

`@import "foo";`

都是合法语法。以下几种情况@import会以css @import规则执行：

1.文件扩展名为.css

2.文件名以`http://`开头

3.文件名是url()

4.@import执行media查询

一个@import语句也可以导入多个文件：

`@import "rounded-corners", "text-shadow";`

2.@extend继承

这个比较niubility，搞得好像是OOP里面的概念。对，@extend就是用于继承别人的属性的。

比如你有一条css这样写：

```
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  border-width: 3px;
}
```
如果想要.seriousError 含有.error的属性，你必须将.seriousError和.error写在一起，像这样：

```
.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd;
}

.seriousError {
  border-width: 3px;
}
```
使用@extend则可以写为：

```
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```
这样，.error 的每个属性都会应用到.seriousError。

* 它是如何工作的？

@extend将当前选择器插入到被继承的选择器后面，使其（使用继承的选择器）拥有被继承的选择器的属性。比较拗口，举个例子来说明：

```
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.error.intrusion {
  background-image: url("/image/hacked.png");
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

我们看到.seriousError里有一个@extend，那么我们说.seriousError是“使用继承的选择器”，而@extend后面的.error是被继承的选择器。那么以后无论在.error里设置什么属性，.seriousError均会拥有它全部的属性。就相当于在.error后面插入一条`.seriousError`。

那么上面的例子会被编译为：

```
.error, .seriousError{
  border: 1px #f00;
  background-color: #fdd;
}
.error.intrusion, .seriousError.intrusion{
  background-image: url("/image/hacked.png");
}
.seriousError {
  border-width: 3px;
}
```

* 多重继承

@extend可以从多个选择器继承。

```
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.attention {
  font-size: 3em;
  background-color: #ff0;
}
.seriousError {
  @extend .error; //继承.error
  @extend .attention; //继承.attention
  border-width: 3px; 
}
```
将被编译为：

```
.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd;
}
.attention, .seriousError {
  font-size: 3em;
  background-color: #ff0;
}
.seriousError {
  border-width: 3px; 
}
```

* 继承链

```
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
.criticalError {
  @extend .seriousError;
  position: fixed;
  top: 10%;
  bottom: 10%;
  left: 10%;
  right: 10%;
}
```

上面的例子中，.criticalError继承.seriousError的属性，而.seriousError又继承.error的属性。此段将被编译为：

```
.error, .seriousError, .criticalError {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError, .criticalError {
  border-width: 3px;
}
.criticalError {
  position: fixed;
  top: 10%;
  bottom: 10%;
  left: 10%;
  right: 10%;
}
```

可见：.criticalError既有.seriousError的属性又有.error的属性。

* 控制语句

1.@if

```
p {
  @if 1 + 1 == 2 { border: 1px solid;  }
  @if 5 < 3      { border: 2px dotted; }
  @if null       { border: 3px double; }
}
```

将被编译为：

```
p {
  border: 1px solid; 
}
```

2.@for

```
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}
```

将被编译为：

```
.item-1 {
  width: 2em; 
}
.item-2 {
  width: 4em; 
}
.item-3 {
  width: 6em; 
}
```

3.@each

```
@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
```

将被编译为：

```
.puma-icon {
  background-image: url('/images/puma.png'); 
}
.sea-slug-icon {
  background-image: url('/images/sea-slug.png'); 
}
.egret-icon {
  background-image: url('/images/egret.png'); 
}
.salamander-icon {
  background-image: url('/images/salamander.png');
}
```

4.@while

```
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}
```

将被编译为：

```
.item-6 {
  width: 12em;
}

.item-4 {
  width: 8em;
}

.item-2 {
  width: 4em; 
}
```

* Mixin规则

Mixin可以定义一个代码块，供以后来调用。

```
@mixin large-text {
  font: {
    family: Arial;
    size: 20px;
    weight: bold;
  }
  color: #ff0000;
}
```

* 调用Mixin

使用@include调用Mixin定义的代码块：

```
.page-title {
  @include large-text;
  padding: 4px;
  margin-top: 10px;
}
```

将被编译为：

```
.page-title {
  font-family: Arial;
  font-size: 20px;
  font-weight: bold;
  color: #ff0000;
  padding: 4px;
  margin-top: 10px; 
}
```

Mixin也可以包含其他Mixin:

```
@mixin compound {
  @include highlighted-background;
  @include header-text;
}

@mixin highlighted-background { background-color: #fc0; }
@mixin header-text { font-size: 20px; }
```

注意：Mixin不可以包含自身，也就是说Mixin不允许递归。

* 参数

Mixin也可以带参数，当有多个参数时，需要将它们放在括号()里面，用逗号`,`隔开。

```
@mixin sexy-border($color, $width) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}

p { @include sexy-border(blue, 1in); }
```

将$color用blue代替，$width用1in代替。

编译结果为：

```
p {
    border-color: blue;
    border-width: 1in;
    border-style: dashed;
}
```

参数也可以带默认值。如果传递参数时没有那个值，则用默认值代替。

```
@mixin sexy-border($color, $width: 1in) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}
p { @include sexy-border(blue); } // 没有传递值给$width, 所以$width用默认值1in
h1 { @include sexy-border(blue, 2in); } //$width: 2in
```

将被编译为：

```
p {
  border-color: blue;
  border-width: 1in;
  border-style: dashed; 
}

h1 {
  border-color: blue;
  border-width: 2in;
  border-style: dashed; 
}
```

参考：http://sass-lang.com/documentation/
