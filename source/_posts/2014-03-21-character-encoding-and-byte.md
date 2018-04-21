---
layout: post
title: '字符编码与字节'
date: 2014-03-21 06:03
comments: true
categories: 计算机基础
---
* 编码系统

字符就是我们看到的”文字“。世界上有很多种语言，比如英语，法语，中文，日语，韩语等等。'abcd',  '我', '&#7843;'等等都是字符。字符是人类理解的文字，而计算机则只能理解二进制位(010101...)，8个二进制位为一个字节(byte)。要将人类理解的字符转换成计算机理解的字节，就需要编码(encode)。英语世界里有一个著名的编码系统[ASCII](http://zh.wikipedia.org/wiki/ASCII "美国信息交换标准代码")，使用一个字节来编码字符。ASCII码一共规定了128个字符的编码，比如空格"SPACE"是32（二进制00100000），大写的字母A是65（二进制01000001）。这128个符号（包括32个不能打印出来的控制符号），只占用了一个字节的后面7位，最前面的1位统一规定为0。但是在非英语语系中，像中文，日语和韩语等语言，一个字节并不能编码所有的字符，这时ASCII编码就不适用了。

不同的国家就创造了自己的编码系统。比如简体中文的GB2312，繁体中文的big5，俄语用koi8-r等。但是不同的编码系统会造成混乱，比如130在法语编码中代表了é，在希伯来语编码中却代表了字母Gimel (ג)。所以如果两个人在交换文件时用的编码却不同就会出现所谓的”乱码“。

这个时候，UNICODE出现了。当当当，救星出世！

Unicode编码系统为表达任意语言的任意字符而设计。它使用4字节的数字来表达每个字母、符号，或者表意文字(ideograph)。每个数字代表 **唯一** 的至少在某种语言中使用的符号。这样一来，每个字符对应一个数字，每个数字对应一个字符。即不存在二义性。不再需要记录“模式”了。U+0041总是代表'A'，即使这种语言没有'A'这个字符。

[http://unicode-table.com/](http://unicode-table.com "Unicode Table")可以查看所有字符的Unicode码。

Unicode又有几种实现形式：

1. UCS-4(UTF-32) 使用4字节编码
2. UCS-2(UTF-16) 使用2字节编码
3. UTF-8 使用1-4字节编码

其中以UTF-8最流行。

UTF-8是一种为Unicode设计的变长(variable-length)编码系统。即，不同的字符可使用不同数量的字节编码。对于ascii字符(A-Z, &c.)utf-8仅使用1个字节来编码。事实上，utf-8中前128个字符(0–127)使用的是跟ascii一样的编码方式。像ñ和ö这样的“扩展拉丁字符(Extended Latin)”则使用2个字节来编码。中文字符比如“中”则占用了3个字节。很少使用的“星芒层字符”则占用4个字节。

这样既可以照顾到所有的字符又能节约存储空间，如果所有的计算机都使用UTF-8编码的话那就“沟通零距离”了。

* Python中的字符与字节

在Python 3中，所有的字符串都是使用Unicode编码的字符序列。utf-8是一种将字符编码成字节序列的方式。编码字符可以使用`str.encode()`，而解码则使用`bytes.decode()`

```python
# 将'我'编码成字节序列(bytes)，使用utf-8
> '我'.encode('utf-8')
b'\xe6\x88\x91'
# 解码
> b'\xe6\x88\x91'.decode('utf-8')
'我'
```

* String vs. Bytes

字节即字节；字符是一种抽象。一个不可变(immutable)的Unicode编码的字符序列叫做string。一串由 **0到255** 之间的数字组成的序列叫做bytes对象。

```python
# 使用“byte字面值”语法b''来定义bytes对象。byte字面值里的每个字节可以是ascii字符或者是从\x00到\xff编码了的16进制数。
> by = b'abcd\x65'

# 一如列表和字符串，可以使用下标记号来获取bytes对象中的单个字节。对字符串做这种操作获得的元素仍为字符串，而对bytes对象做这种操作的返回值则为整数。确切地说，是0–255之间的整数。
> by[0] 
97

# bytes对象是不可变的；我们不可以给单个字节赋上新值。如果需要改变某个字节，
可以组合使用字符串的切片和连接操作(效果跟字符串是一样的)，或者我们也可以将bytes对象转换为bytearray对象。

> by[0] = 102   
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'bytes' object does not support item assignment
> barr = bytearray(by)
> barr[0] = 102 
> barr
bytearray(b'fbcde')
```


参考：[http://sebug.net/paper/books/dive-into-python3/strings.html](http://sebug.net/paper/books/dive-into-python3/strings.html)