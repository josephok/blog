---
layout: post
title: 'jQuery与JavaScript写法对比'
date: 2014-02-13 08:12
comments: true
categories: 前端
---
[jQuery](http://jquery.com/)是一个很流行的JaVascript库，简化了JavaScript的编程。作为学习，我们可以试试将jQuery的一些用法用原生的JavaScript写出来。

1. `$('#container'); //选择id为"container"的元素并生成新的jQuery对象`
JavaScript写法：`var container = document.querySelectorAll("#container");`

2. `$('#container').find('li'); //选择id为"container"元素的后代"li"元素`
JavaScript写法：
`var list = document.querySelectorAll("#container li")`; 

3. `$('a').on('click', fn); //在标签a上绑定事件"click"`
JavaScript写法：
```js
[].forEach.call( document.querySelectorAll('a'), function(el) {
   el.addEventListener('click', function() {
     //function code
  }, false);
 });
```
因为querySelectorAll返回静态的NodeList而非数组，所以我们不能直接使用forEach方法，必须在Array对象上使用forEach


4. `$('ul').on('click', 'a', fn); //绑定事件到"ul"标签，但是只在用户触发其子元素"a"时启动事件处理程序`
JavaScript写法：
```js
document.addEventListener('click', function(e) {
   if ( e.target.matchesSelector('ul a') ) {
      // proceed
   }
}, false);
```

5. `$('#box').addClass('wrap'); //为"#box"添加"wrap"类`
JavaScript写法：
```js
document.querySelector('#box').classList.add('wrap');
```

6. `$('#list').next(); //查找选择"#list"的相邻元素`
JavaScript写法：
```js
var next = document.querySelector('#list').nextSibling;
```

7. `$('\<div id=box>\</div>').appendTo('body'); //"body"标签添加子元素'\<div id=box>\</div>'`
JavaScript写法：
```js
var div = document.createElement('div');
div.id = 'box';
document.body.appendChild(div);
```

8. `$(document).ready(fn);`
JavaScript写法：
```js
document.addEventListener('DOMContentLoaded', function() {
   // have fun
});
```

9. `'.box').css('color', 'red'); //将".box"的style改为：color: red`
JavaScript写法:
```js
[].forEach.call( document.querySelectorAll('.box'), function(el) {
  el.style.color = 'red'; // or add a class
}); //[]也可以写为`Array.prototype`
```

10. `()`
JavaScript写法:
```js
var $ = function(el) {
    return document.querySelectorAll(el);
};
//Usage = $('.box');
```

11. `$("#wrap").empty();//删除"#wrap"的所有子元素`

JavaScript写法:

```js
var parent = document.querySelectorAll("#wrap");
while (parent.firstChild) {
    parent.removeChild(parent.firstChild);
}
```

参考：http://net.tutsplus.com/tutorials/javascript-ajax/from-jquery-to-javascript-a-reference/
