---
layout: post
title: 'Prim算法求最小生成树'
date: 2014-05-10 08:59
comments: true
categories: 
---
如下所示的无向网络包含7个顶点和12条边，边权重之和为243。
![](http://projecteuler.net/project/images/p_107_1.gif)

这个网络还可以用如下的矩阵来表示：

![](http://ww1.sinaimg.cn/large/90b90757gw1eg9ad81a4bj208207mwer.jpg)

但是，在保证网络上所有点之间连通性的前提下，可以将某些边去除掉，从而实现该网络的优化。能够做到最多节省的网络如下所示。其权重和为93，与原网络相比共节省了243 − 93 = 150。

![](http://projecteuler.net/project/images/p_107_2.gif)

[network.txt](http://projecteuler.net/project/network.txt)包含一个拥有四十个顶点的网络，以矩阵形式给出。求在保证连通性的前提下，通过去除边可以做到的最多节省。

这里可以使用Prim算法求出最小生成树，然后求出其权重和。

## Prim算法描述

从单一顶点开始，Prim算法按照以下步骤逐步扩大树中所含顶点的数目，直到遍及连通图的所有顶点。

1. 输入：一个加权连通图，其中顶点集合为V，边集合为E；
2. 初始化：Vnew = {x}，其中x为集合V中的任一节点（起始点），Enew = {}；
3. 重复下列操作，直到Vnew = V：

a) 在集合E中选取权值最小的边（u, v），其中u为集合Vnew中的元素，而v则不是      
	（如果存在有多条满足前述条件即具有相同权值的边，则可任意选取其中之一）；      
b) 将v加入集合Vnew中，将（u, v）加入集合Enew中；

4. 输出：使用集合Vnew和Enew来描述所得到的最小生成树。

具体实现如下：

```python
from collections import defaultdict

# 构造图
# 顶点由'1', '2', '3', ... '40'构成
def build_gragh():
    graph = dict()
    with open('network.txt') as f:
        lines = f.read().splitlines()
        for vh, line in enumerate(lines, start=1):
            g = dict()
            for vs, v in enumerate(line.split(','), start=1):
                try:
                    g[str(vs)] = int(v)
                except ValueError:
                    g[str(vs)] = None
            graph[str(vh)] = g

    return graph

# test gragh
# 由字典表示，两个顶点权重值可以表示为：如A和B的权重为graph['A']['B']
"""
graph = {
    'A': {'B': 16, 'C': 12, 'D': 21},
    'B': {'A': 16, 'D': 17, 'E': 20},
    'C': {'A': 12, 'D': 28, 'F': 31},
    'D': {'A': 21, 'B': 17, 'C': 28, 'E': 18, 'F': 19, 'G': 23},
    'E': {'B': 20, 'D': 18, 'G': 11},
    'F': {'C': 31, 'D': 19, 'G': 27},
    'G': {'D': 23, 'E': 11, 'F': 27}
}
"""

# 求权重和
def sum_weight(graph, duplicate=True):
    sum_ = 0
    for v in graph:
        for vs in graph[v]:
            if graph[v][vs] != None:
                sum_ += graph[v][vs]

    # 如果计算两遍，则除以2
    if duplicate:
        return sum_ // 2
    else:
        return sum_

graph = build_gragh()

# prim算法求最小生成树
def prim_tree(graph):
    # 所有顶点
    vertes = list(graph.keys())
    # 选中顶点
    selected = set()
    # 将第一个顶点加入
    selected.add(vertes[0])
    # 移除已选顶点
    vertes.pop(0)
    
    new_graph = defaultdict(dict)
    # 只要还有未选中顶点，则循环
    while vertes:
        # 假设最小权重为1000
        tmp = 1000
        for u in selected:
            for v in vertes:
                try:
                    if graph[u][v] != None and graph[u][v] < tmp:
                        tmp = graph[u][v]
                        choose_u = u
                        choose_v = v

                except KeyError:
                    pass

        selected.add(choose_v)
        vertes.remove(choose_v)
        new_graph[choose_u][choose_v] = tmp

    return new_graph

print("before: ")
sum1 = sum_weight(graph, duplicate=True)
print(sum1)
print("after:")
# 最小生成树中各条边权重只算一次，所以这里duplicate为False
sum2 = sum_weight(prim_tree(graph), duplicate=False)
print(sum2)

print("saving: {}".format(sum1 - sum2))
```

```bash
$ time python3 solve.py 
before: 
261832
after:
2153
saving: 259679

real	0m0.067s
user	0m0.060s
sys	0m0.004s
```
