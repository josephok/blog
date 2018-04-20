---
layout: post
title: 'DOM与jQuery对象互换'
date: 2014-03-15 03:21
comments: true
categories: 
---
我们知道，DOM对象只能使用原生JS的方法，而jQuery对象只能使用jQuery的方法。如果要在原生JS里使用jQuery对象或者对DOM对象使用jQuery方法的话就必须将二者进行转换。

* DOM对象转为jQuery对象

只需要将DOM对象用`$()`进行包装：
`$(DOMObj)`

*  jQuery对象转为DOM对象

jQuery中介绍了一个`get(index)`的[方法](http://api.jquery.com/get/)

    > Retrieve the DOM elements matched by the jQuery object.

当然也可以使用数组形式的访问，如`$("#selector")[0]`

简单吧^_^