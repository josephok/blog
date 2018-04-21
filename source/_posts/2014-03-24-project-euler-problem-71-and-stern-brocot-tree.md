---
layout: post
title: 'Project Euler Problem 71 and Stern-Brocot tree'
date: 2014-03-24 03:53
comments: true
categories: 数据结构与算法
---
[Project Euler Problem 71](http://projecteuler.net/problem=71) describes as follows:

> Consider the fraction, n/d, where n and d are positive integers. If n < d and HCF(n,d)=1, it is called a reduced proper fraction.

> If we list the set of reduced proper fractions for d ≤ 8 in ascending order of size, we get:
1/8, 1/7, 1/6, 1/5, 1/4, 2/7, 1/3, 3/8, **2/5**, 3/7, 1/2, 4/7, 3/5, 5/8, 2/3, 5/7, 3/4, 4/5, 5/6, 6/7, 7/8

> It can be seen that 2/5 is the fraction immediately to the left of 3/7.

> By listing the set of reduced proper fractions for d ≤ 1,000,000 in ascending order of size, find the numerator of the fraction immediately to the left of 3/7.

Such sequences can be included to be a [Stern–Brocot tree](http://en.wikipedia.org/wiki/Stern%E2%80%93Brocot_tree)

It all starts with fractions and the number line. We start off with the interval [0,1]:

![](http://ww2.sinaimg.cn/large/dd1b49b3jw1efffjbaj8pj205j00qq2p.jpg)

Now let's play a little game. We're going to take the fractions 0/1 and 1/1 and calculate their mediant by taking the sum of their numerators and denominators to get (0+1)/(1+1)=1/2:

![](https://ww4.sinaimg.cn/large/dd1b49b3jw1efffjluy5nj205j00q3ya.jpg)

With this new point on the line, we can do this mediant operation again to get (0+1)/(1+2)=1/3 and (1+1)/(2+1)=2/3:

![](http://ww2.sinaimg.cn/large/dd1b49b3jw1efffjyfjxbj205j015wea.jpg)

Another iteration of this process gives:

![](http://ww2.sinaimg.cn/large/dd1b49b3jw1efffk8knstj205j01e744.jpg)

But we can continue this process indefinitely! And instead of notating these fractions on the number line, we can put them into a tree:

![](http://ww3.sinaimg.cn/large/dd1b49b3jw1efffl3mk2mj206104wt8n.jpg)

Now of course, merely having fractions between zero and one isn't satisfying enough, so we extend the construction of this tree to begin with the points 0/1 and 1/0, as nonsensical as that may sound. Then we get something like this:

![](http://ww2.sinaimg.cn/large/dd1b49b3jw1effflcnw08j20ca05waa9.jpg)

This is the Stern-Brocot tree.

Here we use the subtree which root is 1/2:
![](http://ww3.sinaimg.cn/large/dd1b49b3jw1efffowa90sj20ca05wdg4.jpg)

We need to find a node between 2/5 and 3/7. first, take numerators as [2, 3], denominators as [5, 7]
so we can find a node between 2/5 and 3/7, that is `$\frac {2+3} {5+7} = \frac 5 {12}$`. Then iteratively we use the same strategy to find a node between 5/12 and 3/7. When we reach the condition that denominator > 1000,000, loop terminates.

Here is the code using coffeescript:
```coffeescript
[n, d] = [2, 5]

while d <= 1000000
	n += 3
	d += 7
# go back if we exceeds 
console.log n - 3
```

Reference: [http://hxhl95.github.io/stern-brocot-tree-part-1/](http://hxhl95.github.io/stern-brocot-tree-part-1/)

事实上，可以证明：
如果`$\frac a b < \frac c d$`，则`$\frac a b < \frac {a+c} {b+d} < \frac c d$`
证明如下：
由`$\frac a b ①< \frac c d②$` 可以得到：`$ad < bc$`，那么`$\frac a b = \frac {ab+ad} {b(b+d)} < \frac {ab+bc} {b(b+d)} = \frac{a+c}{b+d}③$` 
即① < ③

且：
`$\frac{a+c}{b+d} = \frac {ad+cd} {d(b+d)} < \frac {bc+cd} {d(b+d)} = \frac c d$`
即③ < ②

`$\therefore ①<③<② that is \frac a b < \frac {a+c}{b+d} < \frac c d$` 