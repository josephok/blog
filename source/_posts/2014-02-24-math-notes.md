---
layout: post
title: 'Math notes'
date: 2014-02-24 04:16
comments: true
categories: Math
---
* 导数

$C' = 0$

$(x^n)' = nx^{n-1}$

$(sinx)' = cosx$

$(tanx)' = \frac {1}{cos^2 x} = sec^2 x$

$(cotx)' = - \frac {1} {sin^2 x} = -csc^2 x$

$(secx)' = secxtanx$

$(cscx)' = -cscxcotx$

$(ln|x|)' = \frac{1}{x}$

$(log_a x)' = \frac{1}{xlna}$

$(e^x)' = e^x$

$(a^x)' = a^xlna, a>0, a\ne1$

$(arcsinx)' = \frac{1}{\sqrt{1-x^2}}$

$(arccosx)' = -\frac{1}{\sqrt{1-x^2}}$

$(arctanx)' = \frac{1}{1+x^2}$

$(arccot x)' = -\frac{1}{1+x^2}$

* 极限

$\lim_{n \to \infty}\frac{ln}{n} = 0$

$\lim_{n \to \infty}\sqrt[n]{n} = 1$

$\lim_{n \to \infty}x^{1/n} = 1(x>0)$

$\lim_{n \to \infty}x^n = 0(|x| < 1)$

$\lim_{n \to \infty}(1+\frac{x}{n})^n = e^x$

$\lim_{n \to \infty}\frac{x^n}{n!} = 0$

* 积分

$\int kx\, dx = kx + C $ (K是常数)

$\int x^u\, dx = \frac {x^{u+1}} {u+1}+ C (u \ne -1)$

$\int \frac{dx} {x} = ln|x| + C$

$\int \frac{1}{1+x^2}dx = \arctan x+C$

$\int \frac{1}{\sqrt {1-x^2}}dx = \arcsin x+C$

$\int \cos x dx = \sin x+C$

$\int \sin x dx = -\cos x+C$

$\int \sec^2x dx = -\tan x+C$

$\int \csc^2x dx = -\cot x+C$

$\int \sec x \tan x dx = \sec x+C$

$\int e^x dx = e^x+C$

$\int a^x dx = \frac {a^x} {lna}+C$

$\int u\,dv=uv-\int v\,du$

* 费马小定理

假如a是一个整数，p是一个质数，那么$a^p - a$是p的倍数，可以表示为
$a^p \equiv a \pmod{p}$
如果a不是p的倍数，这个定理也可以写成
$a^{p-1} \equiv  1 \pmod{p}$

* 微积分基本定理

(1) 设$f$为定义在闭区间$[a,b]$的实函数
$F(x) = \int_a^x f(t)\, dt\,　 $ ($x \in [a,b]$)
那么，$F(x)$可导，及$F'(x)=f(x)$
即：$\frac d{dx}\int_a^xf(t)dt=f(x)$

(2) 设 $f$ 为在闭区间 $[a, b]$ 内连续的实函数，及设 $F$ 为 $f$ 的一个原函数：
$F'(x) = f(x)\,　$　 $x \in [a,b]$
那么
$\int_a^b f(x)\,dx = F(b) - F(a)$


* 无穷级数收敛性判断：

![](http://ww3.sinaimg.cn/large/dd1b49b3jw1efhhbuijsuj21010id422.jpg)
![](http://ww3.sinaimg.cn/large/dd1b49b3jw1efhhbsj4m9j20zz0gf779.jpg)

* 常数e

$$
e=\lim_{n\to\infty}\left(1+\frac{1}{n}\right)^n
e=\sum_{n=0}^\infty \frac{1}{n!}
$$

* 欧拉公式

$$
e^{i\theta}=\cos\theta+i\sin\theta
$$

* 斯特灵公式

$$
n! \approx \sqrt{2\pi n}\, \left(\frac{n}{e}\right)^{n}
$$

* 正态分布

均值为$\mu$, 方差为$\sigma^2$ (或标准差$\sigma$)
正态分布的概率密度函数:
$$f(x;\mu,\sigma)
=\frac{1}{\sigma\sqrt{2\pi}} \, \exp \left( -\frac{(x- \mu)^2}{2\sigma^2} \right)
$$

* 叶斯定理：
$$
P(A_i|B) = \frac{P(B | A_i)\, P(A_i)}{\sum_j P(B|A_j)\,P(A_j)}
$$
