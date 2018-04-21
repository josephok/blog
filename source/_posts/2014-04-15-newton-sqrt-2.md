---
layout: post
title: '利用牛顿法求2的平方根'
date: 2014-04-15 08:38
comments: true
categories:
---
牛顿法（Newton's method）又称为牛顿-拉弗森方法（Newton-Raphson method），它是一种在实数域和复数域上近似求解方程的方法。方法使用函数f(x)的泰勒级数的前面几项来寻找方程f(x)=0的根。

牛顿法的步骤：

1. 猜方程$f(x)=0$的解的第一个近似值
2. 利用公式

$$
x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}
$$
从第一次近似求得第二次近似，再从第二次近似求得第三次近似...，迭代下去就可以求出比较精确的值。

## 例子(求2的平方根)

求方程$f(x)=x^2-2=0$的正根

解：由于
$$
f(x)=x^2-2, f'(x)=2x =>
x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}=x_n-\frac {x_n^2-2}{2x_n}
$$
$$
x_{n+1}=x_n-\frac{x_n}2+\frac 1 x_n=\frac{x_n} 2 + \frac 1 x_n
$$

我们从$x_0=1$开始迭代：
```coffeescript
x0 = 1
for i in [0..10]
    x = x0/2.0 + 1.0/x0
    x0 = x

console.log x
```

求得结果为：
```bash
$ coffee newton.coffee
1.414213562373095
```
更一般的可以推广到求$\sqrt[b]{a}$的值。

$$
f(x)=x^b-a, \\\\
f'(x)=bx^{b-1}, \\\\
x_{n+1}=x_n - \frac {x_n^b-a}{bx_n^{b-1}}=\frac{b-1}bx_n+\frac a{bx_n^{b-1}}
$$

```coffeescript
nth_root = (a, b) ->
    x0 = 1
    for i in [0..10]
        x = (b-1.0)/b * x0 + a/(b*Math.pow(x0, b-1))
        x0 = x
    x
# sqrt(3)
console.log nth_root(3, 2)
# output: 1.7320508075688772
# 3^(1/3)
console.log nth_root(3, 3)
# output: 1.4422495703074083
```
