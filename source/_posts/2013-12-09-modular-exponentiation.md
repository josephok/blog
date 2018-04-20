---
layout: post
title: '同余幂求解'
date: 2013-12-09 09:41
comments: true
categories:
---
在密码学中重要的是能有效地求$ b^n mod (m) $, 其中$b, n, m$都是大整数。先计算$b^n$，再求$b^n$除以$m$的余数是不可行的，因为$b^n$是非常大的数。可行的是一种利用指数n的二进制展开的算法。这个算法依次求
$$
b mod (m), b^2 mod (m), b^4 mod (m), ..., b^{2^{k-1}} mod (m)
$$
把其中$a_j = 1$的那些项$b^{2^j} mod(m)$相乘，在每次乘法后求乘积除以m的余数。

算法伪代码如下：

```
x = 1
power = b mod(m)
for i = 0 to k-1
begin
if a_i = 1 then x = x * power mod(m)
power = power * power mod(m)
end
```
