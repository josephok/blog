---
layout: post
title: '判断某点是否在三角形内部'
date: 2014-04-29 11:50
comments: true
categories: 数据结构与算法
---
Project Euler第102题：

> Three distinct points are plotted at random on a Cartesian plane, for which -1000 ≤ x, y ≤ 1000, such that a triangle is formed.

>Consider the following two triangles:

>A(-340,495), B(-153,-910), C(835,-947)

>X(-175,41), Y(-421,-714), Z(574,-645)

>It can be verified that triangle ABC contains the origin, whereas triangle XYZ does not.

>Using triangles.txt (right click and 'Save Link/Target As...'), a 27K text file containing the co-ordinates of one thousand "random" triangles, find the number of triangles for which the interior contains the origin.

>NOTE: The first two examples in the file represent the triangles in the example given above.

大致意思是给定3个点A, B, C，判断A, B, C组成的三角形是否包含原点P(0, 0)，或者说原点是否在三角形ABC的内部。
解答这道题的思路是判断ABC的面积是否等于PAB, PBC, PAC三个面积之和。

![](https://ww4.sinaimg.cn/large/90b90757gw1efwprg17zjj20dd0a3jrr.jpg)
P在三角形内部

![](https://ww1.sinaimg.cn/large/90b90757gw1efwq0l6wrzj20em0ahmxl.jpg)
P在三角形外部

如何计算三角形面积？
在这里，三个点A, B, C分别用坐标表示。如A(-340,495), B(-153,-910), C(835,-947)，[维基百科](http://en.wikipedia.org/wiki/Triangle#Using_coordinates)上给出了计算公式：

$$
T = \frac{1}{2} \big| (x_A - x_C) (y_B - y_A) - (x_A - x_B) (y_C - y_A) \big|
$$
其中，$x_A$表示$A$的$x$坐标，$y_B$表示$B$的$y$坐标...


Python实现：

```python
def main():
    with open('triangles.txt') as f:
        lines = f.read().splitlines()

    counts = 0
    for line in lines:
        points = line.split(',')
        points = [int(point) for point in points]
        A = points[:2]
        B = points[2:4]
        C = points[4:]
        P = (0, 0)
        if area(A, B, C) == area(P, B, C) + area(P, A, C) + area(P, A, B):
            counts += 1
    print(counts)

# 求面积，根据上面的公式
def area(A, B, C):
    return abs((A[0]-C[0]) * (B[1]-A[1]) - (A[0]-B[0]) * (C[1]-A[1]))

if __name__ == '__main__':
    main()
```

```bash
$ time python solve.py
228

real	0m0.038s
user	0m0.036s
sys	0m0.000s
```