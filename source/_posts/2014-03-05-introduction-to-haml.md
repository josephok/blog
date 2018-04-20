---
layout: post
title: 'Haml简介'
date: 2014-03-05 03:47
comments: true
categories: 
---
[Haml](http://haml.info/)是X **H**TML **A**bstraction **M**arkup **L**anguage的缩写，用Ruby写成，可以将写成的模版转换为HTML代码。Haml的优点是简洁省去了HTML很多繁琐的标签。

### 安装Haml:

```bash
gem install haml
```

### Haml语法

* HTML5 DOCTYPE
```
!!! 5
```
HTML：
```html
<!DOCTYPE html> 
```

* XHTML Strict DOCTYPE
```
!!! Strict 
```
HTML:
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

* XML DOCTYPE
```
!!!XML
```
HTML:
```html
<?xml version='1.0' encoding='utf-8' ?>
```

* 标签
`%`开头表示标签，如`%html`, `%head`, `%body`等。**代码块的层级关系用缩进表示。**
```
!!!5
%html
	%head
		%title Haml introduction
	%body
		give me five!
```
输出HTML为：
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Haml introduction</title>
  </head>
  <body>
    give me five!
  </body>
</html>
```

* ID
在ID前加上`#`即可：
```
%div#mine
%p#yours
```

HTML:
```html
<div id="mine"></div>
<p id="yours"></p>
```

* Class
在Class前加`.`：
```
%div.mine
%p.yours
```
HTML:
```html
<div class="mine"></div>
<p class="yours"></p>
```

还可以添加多个Class:
```
%div.mine.yours
```
HTML:
```html
<div class="mine yours"></div>
```

`div`是默认标签，如果只写ID或者Class，那么就默认是div标签：
```
#mine
```
HTML:
```html
<div id='mine'></div>
```

ID和Class可以混用：
```
#mine.yours
#mine.yours.his
```
HTML:
```html
<div class='yours' id='mine'></div>
<div class='yours his' id='mine'></div>
```

* 属性

属性可以使用Ruby Hash语法表示：
```
%a{:href => 'http://hi.com'} hi
%input{:type => 'submit'}
```
HTML:
```html
<a href='http://hi.com'>hi</a>
<input type='submit'>
```
或者可以写成：
```
%a(href='http://hi.com') hi
%input(type='submit')
```

* 使用数组表示ID和Class:
```
%p{:class => ['one','two']} hi
%p{:id=> ['one','two']} hi
```
HTML:
```html
<p class='one two'>hi</p>
<p id='one_two'>hi</p>
```

* 注释

"/"起首的行表示HTML注释，"-#"起首的行表示Haml注释。还可以添加条件注释：
```
/[if IE] %link { :rel => "stylesheet", :href => "/css/ie.css" } 
```
HTML:
```html
<!--[if IE]> <link href="/css/ie.css" rel="stylesheet"> <![endif]-->
```

### 编译

```
haml input.haml output.html
```

更详细语法的请参考：
[http://haml.info/docs/yardoc/file.REFERENCE.html](http://haml.info/docs/yardoc/file.REFERENCE.html)

[HAML Cheat Sheet](http://www.cheatography.com/specialbrand/cheat-sheets/haml/)