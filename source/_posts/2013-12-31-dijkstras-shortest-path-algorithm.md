---
layout: post
title: '最短路径算法(Dijkstra’s shortest path algorithm)'
date: 2013-12-31 09:39
comments: true
categories: 数据结构与算法
---
算法描述：

给定一个图和一个起点，找出该起点到图中其他各顶点的最短路径。Dijkstra算法是一种[贪心算法](http://en.wikipedia.org/wiki/Greedy_algorithm)。

Dijkstra算法和[Prim算法](http://en.wikipedia.org/wiki/Prim%27s_algorithm)非常相似。我们可以生成一棵以起点为根的最短路径树(shortest path tree)。然后我们维护2个集合：一个集合是最短路径树中的所有顶点(假设为A)；另一个集合是其他顶点(假设为B)。
通过迭代，每次在B中找一个到起点距离最短的顶点，加入到A中；当B为空集时，算法结束。

下面是具体的算法：

(1) 创建一个sptSet (最短路径树集合)，初始化该集合为空集。
(2) 给图中所有顶点赋值一个距离，初始化起点为0，其他顶点为INFINITE(无穷大&#8734;)
(3)只要sptSet没有包含所有顶点：
a)从B中选一个顶点u, u的距离值最小。
b)将u加入sptSet.
c)更新u的邻接顶点的距离值，对于每一个邻接顶点v, 如果u的距离值加上边u-v的权值小于v的距离值，那么就更新v的距离值。(更新为u的距离值加上u-v的权值)

来看一个具体的例子:

选取顶点0为起始点。sptSet现在是空集，各顶点的距离值分别是{0, &#8734;, &#8734;, &#8734;, &#8734;, &#8734;, &#8734;, &#8734;}
顶点0加入到sptSet集合中，sptSet为{0}，然后更新顶点0的邻接顶点1和7，此时1的距离值为4, 7的距离值为8。
sptSet中的顶点显示为绿色：(&#8734;的顶点未予显示)
选取距离值最小但不在sptSet集合中的顶点，因为1的距离值最小，所以将1加入sptSet中，现在sptSet变为{0, 1}。更新1的邻接顶点的距离值：2的距离值变为4+8=12
 现在继续选取距离值最小但不在sptSet集合中的顶点，此时7为最小，所以选取7，sptSet变为{0, 1, 7}。接着更新7的邻接顶点：6的距离值变为8+1=9，8的距离值变为8+7=15
现在选取顶点6，sptSet变为{0, 1, 7, 6}。接着更新6的邻接顶点：5的距离值变为9+2=11，8的距离值为9+6=15
现在选取顶点5，sptSet变为{0, 1, 7, 6, 5}，更新5的邻接顶点：2为11+4=15>12故不更新，3为11+14=25，4为11+10=21
![](http://ww3.sinaimg.cn/large/90b90757gw1ec50hbzue9j20hl08k74j.jpg)
现在选取顶点2，sptSet变为{0, 1, 7, 6, 5, 2}，更新2的邻接顶点：3为12+7=19\<25 故更新3的距离值为19，8为12+2=14\<15 故更新8的距离值为14，5为12+4=16>11故不更新。
![](https://ww1.sinaimg.cn/large/90b90757gw1ec50rnhr0vj20hl08kglv.jpg)
现在选取顶点8，sptSet变为{0, 1, 7, 6, 5, 2, 8}，8的邻接顶点：2, 7, 6不用更新。
![](https://ww1.sinaimg.cn/large/90b90757gw1ec50wqfo85j20hl08kmxf.jpg)
现在选取顶点3，sptSet变为{0, 1, 7, 6, 5, 2, 8, 3}，3的邻接顶点2, 5, 4不用更新。
![](http://ww3.sinaimg.cn/large/90b90757gw1ec511amgwbj20hl08kq37.jpg)
最后选取顶点4，sptSet变为{0, 1, 7, 6, 5, 2, 8, 3, 4}，此时已经包含图中所有顶点，故为最终结果。
![](https://ww1.sinaimg.cn/large/90b90757gw1ec511vb6c5j20hl08kq37.jpg)

C语言实现：
我们使用一个boolean数组sptSet[]表示SPT中的顶点，如果stpSet[v]=true, 则顶点v在SPT中。数组dist[]存贮所有顶点的最短路径值。

```c
// A C / C++ program for Dijkstra's single source shortest path algorithm.
// The program is for adjacency matrix representation of the graph

#include <stdio.h>
#include <limits.h>
typedef enum {false, true}bool;
// Number of vertices in the graph
#define V 9

// A utility function to find the vertex with minimum distance value, from
// the set of vertices not yet included in shortest path tree
int minDistance(int dist[], bool sptSet[])
{
   // Initialize min value
   int min = INT_MAX, min_index;

   for (int v = 0; v < V; v++)
     if (sptSet[v] == false && dist[v] <= min)
         min = dist[v], min_index = v;

   return min_index;
}

// A utility function to print the constructed distance array
int printSolution(int dist[], int n)
{
   printf("Vertex   Distance from Source\n");
   for (int i = 0; i < V; i++)
      printf("%d \t\t %d\n", i, dist[i]);
}

// Funtion that implements Dijkstra's single source shortest path algorithm
// for a graph represented using adjacency matrix representation
void dijkstra(int graph[V][V], int src)
{
     int dist[V];     // The output array.  dist[i] will hold the shortest
                      // distance from src to i

     bool sptSet[V]; // sptSet[i] will true if vertex i is included in shortest
                     // path tree or shortest distance from src to i is finalized

     // Initialize all distances as INFINITE and stpSet[] as false
     for (int i = 0; i < V; i++)
        dist[i] = INT_MAX, sptSet[i] = false;

     // Distance of source vertex from itself is always 0
     dist[src] = 0;

     // Find shortest path for all vertices
     for (int count = 0; count < V-1; count++)
     {
       // Pick the minimum distance vertex from the set of vertices not
       // yet processed. u is always equal to src in first iteration.
       int u = minDistance(dist, sptSet);

       // Mark the picked vertex as processed
       sptSet[u] = true;

       // Update dist value of the adjacent vertices of the picked vertex.
       for (int v = 0; v < V; v++)

         // Update dist[v] only if is not in sptSet, there is an edge from
         // u to v, and total weight of path from src to  v through u is
         // smaller than current value of dist[v]
         if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX
                                       && dist[u]+graph[u][v] < dist[v])
            dist[v] = dist[u] + graph[u][v];
     }

     // print the constructed distance array
     printSolution(dist, V);
}

// driver program to test above function
int main()
{
   /* Let us create the example graph discussed above */
   int graph[V][V] = {{0, 4, 0, 0, 0, 0, 0, 8, 0},
                      {4, 0, 8, 0, 0, 0, 0, 11, 0},
                      {0, 8, 0, 7, 0, 4, 0, 0, 2},
                      {0, 0, 7, 0, 9, 14, 0, 0, 0},
                      {0, 0, 0, 9, 0, 10, 0, 0, 0},
                      {0, 0, 4, 0, 10, 0, 2, 0, 0},
                      {0, 0, 0, 14, 0, 2, 0, 1, 6},
                      {8, 11, 0, 0, 0, 0, 1, 0, 7},
                      {0, 0, 2, 0, 0, 0, 6, 7, 0}
                     };

    dijkstra(graph, 0);

    return 0;
}
```

输出：

```
Vertex   Distance from Source
0                0
1                4
2                12
3                19
4                21
5                11
6                9
7                8
8                14
```


时间复杂度是\\( O(V^2)\\)，Dijkstra算法不适用于带负权值的图。