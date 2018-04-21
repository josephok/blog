---
layout: post
title: 'Python学习笔记'
date: 2014-02-12 01:48
comments: true
categories: 编程语言
tags: Python
---
* 内置数据类型

1. Booleans【布尔型】 或为 True［真］ 或为 False［假］。
2. Numbers【数值型】 可以是 Integers【整数】（1 和 2）、Floats【浮点数】（1.1 和 1.2）、Fractions【分数】（1/2 和 2/3）；甚至是 Complex Number【复数】。
3. Strings【字符串型】 是 Unicode 字符序列，例如： 一份 html 文档。
4. Bytes【字节】 和 Byte Arrays【字节数组】， 例如: 一份 jpeg 图像文件。
5. Lists【列表】 是值的有序序列。
6. Tuples【元组】 是有序而不可变的值序列。
7. Sets【集合】 是装满无序值的包裹。
8. Dictionaries【字典】 是键值对的无序包裹。

* 内置异常类型

1. **BaseException**: 所有内建异常的基础类。
2. **Exception**: 所有的内建的、非系统预置的异常均继承这个类，所有用户定义的异常也应该继承这个类。
3. **StandardError**: 所有内建异常的基础类，除了StopIteration, GeneratorExit, KeyboardInterrupt和SystemExit。 StandardError 是继承Exception而来的。
4. **ArithmeticError**: 一些内建算术错误异常的基类，如： OverflowError, ZeroDivisionError,FloatingPointError.
5. **LookupError**: 当一个键值或索引在数组或序列中无效时所触发的所有异常的基类: IndexError,KeyError. 它也会由sys.setdefaultencoding()直接触发。
6. **EnvironmentError**: 所有能发生在Python系统之外的异常的基类:IOError, OSError.
7. **AssertionError**: 当判定条件失败时，触发此异常。
8. **AttributeError**: 当一个属性被引用或赋值时出现错误会引发此异常（当一个对象不支持属性被引用或赋值时，会触发TypeError异常）
9. **EOFError**: 当内建函数(`input() 或 raw_input()`) 达到文件尾时触发此异常。
10. **FloatingPointError**: 浮点操作失败时触发此异常。
11. **GeneratorExit**: 当调用生成器的close() 方法时，触发此异常。它直接继承了Exception 用于替代 StandardError ，毕竟这是一个技术手段并不是一个错误异常。
12. **IOError**: 当I/O操作(如一个 print 语句、内建 open()函 数或调用文件对象的某个方法)因为I/O相关的问题而失败时触发此异常，例如：“无此文件”或“没有足够的磁盘空间”。这个类继承于EnvironmentError。
13. **ImportError**: 当 import 语句无法找到对应的模块定义或from…import 无法找到对应名字的内容时触发此异常。
14. **IndexError**: 当一个序列子集超出范围时触发此异常。(索引会被截取以保证在合理的范围内； 如果索引x不是一个整数， TypeError异常会被触发)
15. **KeyError**: 当键值并不存在于图（字典）中，会触发此异常。
16. **KeyboardInterrupt**: 当用户按下终止键时触发此异常（通常是Ctrl+C或者Delete键）。
17. **MemoryError:** 当某些操作导致内存耗尽但应能恢复的情况下（通过删除一些对象来释放内存），触发此异常。
18. **NameError**: 当无法找到对应名字的本地变量或全局变量时，触发此异常。这只针对无效的名字。 附带参数是包含了无法找到的名字的错误信息。
19. **NotImplementedError**: 这个异常继承于 RuntimeError. 用户定义基类后，抽象方法可以触发这个异常来要求派生类必须实现该抽象方法。
20. **OSError**: 这个类继承于 EnvironmentError ，主要用于os模块的os.error异常。
21. **OverflowError**: 当一个算术运算太大导致数值溢出时触发此异常。
22. **ReferenceError**: weakref.proxy()函数产生一个弱引用代理时，此异常被触发。弱引用代理通常访问一个被引用对象的属性，但这个对象已经被垃圾回收。更多的弱引用信息，请参考weakref模块。
23. **RuntimeError**: 当一个无法分类的错误发生时，触发该异常。
24. **StopIteration**: 当一个迭代器的next()方法无法获得更多的值时，触发该异常。
25. **SyntaxError**: 当语法解析器遇到语法错误时触发此异常。
26. **SystemError**: 当解释程序遇到一个内部错误，但是情况看来可以纠正，不需要放弃退出。辅助参数是一个字符串，标明在更顶层什么出错了。
27. **SystemExit**: 这个异常被 sys.exit() 函数触发。当这个异常没有被有效处理，Python终止程序并退出；没有堆栈信息打印。如果辅助参数是整数，它表示系统退出状态（和C语言的exit() 函数类似）；如果它是空则退出状态为0；如果它是其它类型（例如字符串），这个对象会被打印输出，并且退出状态为1。
28. **TypeError**: 当一个操作或函数调用一个不恰当的对象类型时，触发此异常。附带参数是一个字符串，表明具体的不匹配的类型。
29. **UnboundLocalError**: 当在一个函数或方法内引用本地变量但变量并没有赋值时，触发此异常。
30. **UnicodeDecodeError**: 当一个Unicode相关的编码、解码错误发生时，触发此异常。它是ValueError的子类。
31. **UnicodeEncodeError**: 在文字编码时发生一个Unicode相关的错误则触发此异常。它是UnicodeError的子类。
32. **UnicodeError**: 在文字解码时发生一个Unicode相关的错误则触发此异常。它是UnicodeError的子类。
33. **UnicodeTranslateError**: 在转换文字编码时，当一个Unicode相关的错误发生，触发此异常。它是UnicodeError的子类。
34. **ValueError**: 当一个内建操作或函数接收了参数，参数的类型是对的但值并不符合并且无法匹配一个更加精准的异常（例如：IndexError）， 触发此异常。
35. **WindowsError**: 当Windows系统下特定的错误发生或当错误编号无法映射 errno值时，触发此异常。
36. **ZeroDivisionError**: 当除法或取余操作分母为0时，触发此异常。附带的参数是一个文字信息，标明了运算类型和具体运算数据。

异常处理详解：[http://www.w3cschool.cc/python/python-exceptions.html](http://www.w3cschool.cc/python/python-exceptions.html)

* Python yield

带有 yield 的函数在 Python 中被称之为 generator（生成器）。
用yield生成Fibonacci序列：

```python fibonacci.py
def fib(max): 
    n, a, b = 0, 0, 1 
    while n < max: 
        yield b 
        a, b = b, a + b 
        n = n + 1
        
for i in fib(5):
	print(i)

	
1
1
2
3
5
```

yield 的作用就是把一个函数变成一个 generator，带有 yield 的函数不再是一个普通函数，Python 解释器会将其视为一个 generator，调用 fib(5) 不会执行 fib 函数，而是返回一个 iterable 对象！在 for 循环执行时，每次循环都会执行 fib 函数内部的代码，执行到 yield b 时，fib 函数就返回一个迭代值，下次迭代时，代码从 yield b 的下一条语句继续执行，而函数的本地变量看起来和上次中断执行前是完全一样的，于是函数继续执行，直到再次遇到 yield。
调用fib函数可以看到其返回一个generator object:

```python
>>> fab(5)
<generator object fib at 0x010DEE40>
```

* import

使用import导入模块，可以自定义模块，模块名即文件名不含后缀。在程序中使用import时，Python解释器会依照特定路径搜索。可以使用sys.path查看：
```python
>>> import sys
>>> sys.path
['', 'C:\\Python33\\Lib\\idlelib', 'C:\\WINDOWS\\system32\\python33.zip', 'C:\\Python33\\DLLs', 'C:\\Python33\\lib', 'C:\\Python33', 'C:\\Python33\\lib\\site-packages']
>>> sys.path[0]
''
```
sys.path[0]表示当前目录。

* `__name__` 变量

由于主程序代码无论模块是被导入还是被直接执行都会运行，我们必须知道模块如何决定运行方向。一个应用程序可能需要导入另一个应用程序的一个模块，以便重用一些有用的代码。如果模块是被导入， `__name__` 的值为模块名字；如果模块是被直接执行， `__name__` 的值为 '__main__' 

* 运算符优先级(从高到低)

|运算符|说明|
|----|---|
|`(...), [...], {...}`|创建元组,列表,字典|
|\`...\`|字符串转换|
|`s[i]`, `s[i:j]`, `.attr`|索引,切片,属性|
|`f(...)`|函数调用|
|`+x` , `-x` , `~x`  |                          一元运算符|
|`x ** y` |                                 乘方|
|`x * y` , `x / y` , `x % y`  |                 乘,除,取模|
|`x + y` , `x - y`                         | 加,减|
|`x << y` , `x >> y`  |                       移位|
|`x & y`|                                   按位与|
|`x ^ y`|                                   按位异或|
|按位或|                                   按位或|
|`in`, `not in`, `is`, `is not`, `<`, `<=`, `>`, `>=`, `!=`, `==` |比较,身份,序列成员检测|
|`not x`      |                             逻辑非|
|`x and y`  |                               逻辑与|
|`x or y`   |                               逻辑或|
|`lambda args : expr`       |               lambda函数表达式|

* 类成员访问控制

Python并不支持正式的访问控制。但是有一个惯例，那就是以单下划线`_`开头的成员被认为是protected的(本类和子类是可以访问的)，以双下划线`__`开头的成员被认为是private的(只在本类可以访问)。

* `__slots__`用法

一般来说，一个实例可以动态地添加任意新的属性，Python中每个实例均有一个`__dict__`属性，其为字典类型，它保存着实例的所有属性。如果不想在实例化时生成`__dict__`，我们可以使用`__slots__`。具体做法是在类中申明`__slots__ = (属性1, 属性2, 属性3...)`，那么类的实例对象便不能再添加新的属性。
举例说明：
先不用`__slots__`
```python
class Person:
	def __init__(self, name, age, height):
		self._name = name
		self._age = age
		self._height = height

zhangsan = Person('zhangsan', 21, 180)
```
查看`__dict__`：
```python
>>> zhangsan.__dict__
{'_name': 'zhangsan', '_height': 180, '_age': 21}
```
动态添加成员变量：
```python
>>> zhangsan._weight = 65
>>> zhangsan.__dict__
{'_name': 'zhangsan', '_weight': 65, '_height': 180, '_age': 21}
```
如果使用`__slots__`：

```python
class Person:
	__slots__ = ('_name', '_age', '_height')
	def __init__(self, name, age, height):
		self._name = name
		self._age = age
		self._height = height

zhangsan = Person('zhangsan', 21, 180)
```

再来查看`__dict__`：
```python
>>> zhangsan.__dict__
Traceback (most recent call last):
  File "<pyshell#20>", line 1, in <module>
    zhangsan.__dict__
AttributeError: 'Person' object has no attribute '__dict__'
```

添加成员变量：
```python
>>> zhangsan._weight = 65
Traceback (most recent call last):
  File "<pyshell#21>", line 1, in <module>
    zhangsan._weight = 65
AttributeError: 'Person' object has no attribute '_weight'
```
可见使用`__slots__`之后类中的成员变量便不可再更改。
什么时候使用`__slots__`？
一般情况下不使用`__slots__`，只有在生成大量实例并且实例的成员变量固定的情况下使用`__slots__`可以节约内存。(不用保存`__dict__`)
这里有一个例子来说明节约内存的情况：[http://tech.oyster.com/save-ram-with-python-slots/](http://tech.oyster.com/save-ram-with-python-slots/)

文章结尾有一条：
**Warning: ** Don’t prematurely optimize and use this everywhere! It’s not great for code maintenance, andit really only saves you when you have thousands of instances.