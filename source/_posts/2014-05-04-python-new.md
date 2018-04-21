---
layout: post
title: 'Python __new__解释'
date: 2014-05-04 06:10
comments: true
categories: 编程语言
tags: Python
---
Python实例化对象操作过程，首先是调用`__new__()`方法创建实例，然后调用`__init__()`方法初始化对象。一般情况下，定义类时不需要显式的重写`__new__()`方法；但是某些情况下，比如继承immutable类型(如int, str, tuple, frozenset)时，因无法在`__init__()`中修改其元素，所以需要重写`__new__()`。

```python
import math

class Point(tuple):
		#第一个参数是类型
    def __new__(cls, x, y):
        return tuple.__new__(cls, (x, y))

    def distance(self, other):
        return math.sqrt((self[0]-other[0]) ** 2 + (self[1]-other[1]) ** 2)

    def __repr__(self):
        return "Point({0}, {1})".format(self[0], self[1])

p = Point(1, 1)
p2 = Point(0, 0)
p3 = Point(3, 4)
print(p.distance(p2))
print(p2.distance(p3))
print(repr(p))
```

`__new__()`返回某个类的实例对象，而`__init__()`不返回任何值，仅做初始化操作。