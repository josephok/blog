---
layout: post
title: 'JavaScript数据类型转换'
date: 2014-02-28 09:47
comments: true
categories: 前端
---
* 非数值类型转换为数值类型

有三个函数可以实现：
`Number()`
`parseInt()`
`parseFloat()`

Number()函数遵循以下规则:

1. true和false分别转换为1和0
2. 如果参数是数值则不变
3. null转换为0
4. undefined 转换为NaN.
5. 如果参数是字符串，则按照以下规则:

a. 如果只含数字，或(+-)符号，则转为相应的十进位数值，开头的"0"将被忽略：

```js
> Number('-65')
-65
> Number('100')
100
> Number('00100')
100
```

b. 如果含有合法的浮点数值，如"1.2345"等，则转为此浮点数，忽略开头的"0"。

```js
> Number('001.2345678')
1.2345678
> Number('-001.2345678')
-1.2345678
```

c. 如果含有合法的16进位数值，则转换为相应的十进制数值：

```js
> Number('0xffff')
65535
```

d. 空字符串转换为0，包括只含空格的字符串：

```js
> Number(' ')
0
> Number('     ')
0
> Number('')
0
```

e. 如果该字符串是其他格式，则返回NaN：
```js
> Number('h')
NaN
```


6.  当参数是对象时，调用对象的valueOf方法并按照上述规则转换，如果结果是`NaN`则调用toString()方法。

`parseInt()`将字符串转换为整型，规则为：如果首字符不是数字，+-符合，则返回`NaN`。空字符串会被转为`NaN`：

```js
> parseInt('')
NaN
> parseInt(' ')
NaN
```

如果首字符是数值，+-符号，那么就往下找，直到遇到非数值字符：

```js
> parseInt('+ji')
NaN
> parseInt('+1ji')
1
> parseInt('-1ji')
-1
> parseInt('1234ji')
1234
> parseInt('1234.3333')
1234
```

`parseInt()`还可以接受一个基数参数(radix)，表示按照多少进制转换：

```js
> parseInt('ffff', 16)
65535
> parseInt('110', 2)
6
> parseInt('110', 4)
20
> parseInt('110', 6)
42
> parseInt('110', 8)
72
> parseInt('110', 16)
272
> parseInt('110', 20)
420
```

`parseFloat()`工作方式和`parseInt()`类似，它可以接受一个字符串中有一个小数点，第二个小数点将被忽略：

```js
> parseFloat('1.23')
1.23
> parseFloat('1.23.45')
1.23
```

`parseFloat()`只能转换10进制数，不能带基数参数(radix)。

* 非字符串转换为字符串

非字符串可以通过`toString()`方法 **显示** 转换为字符串类型。

```js
> 2.toString（）
SyntaxError: Unexpected token ILLEGAL
这是JavaScript 解析器的一个错误， 它试图将点操作符解析为浮点数字面值的一部分。有很多变通方法可以让数字的字面值看起来像对象。

2..toString(); // 第二个点号可以正常解析
2 .toString(); // 注意点号前面的空格
(2).toString(); // 2先被计算
```

`toString()`方法在转换数值类型时可以加上`radix`参数：

```js
var num = 10;
alert(num.toString());       //”10”
alert(num.toString(2));      //”1010”
alert(num.toString(8));      //”12”
alert(num.toString(10));     //”10”
alert(num.toString(16));     //”a”
```

null和undefined没有toString()方法。这时我们可以使用String()函数:

```js
> String(null)
"null"
> String(undefined)
"undefined"
```

* 转换成布尔值

使用Boolean方法，可以将任意类型的变量转为布尔值。以下六个值的转化结果为false，其他的值全部为true。

>1. undefined
2. null
3. -0
4. +0
5. NaN
6. ''（空字符串）

```js
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean(NaN) // false
Boolean('') // false
```

请注意，空对象{}和空数组[]都会被转成true。

```js
Boolean([]) // true
Boolean({}) // true
```

所有对象的布尔值都是true，甚至连false对应的布尔对象也是true。

```js
Boolean(new Boolean(false))
// true
```