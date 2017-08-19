---
title: CMU 15650 - Bipartite Matching
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-11-02 07:48:45
---
CMU 15650 - Algorithm and Data Structure 

网络流问题
Bipartite Matching

reference:
1. [Maximum Bipartite Matching](http://www.geeksforgeeks.org/maximum-bipartite-matching/)
2. [二分图的最大匹配](http://blog.csdn.net/pi9nc/article/details/11848327)

<!-- more -->

***

# Problem 
**Maximum Bipartite Matching：**
Given a bipartite graph $G = (A ∪ B , E)$, find an $S ⊆ A × B$ that is a matching and is as large as possible.

1. 如果所有的顶点都能被匹配，那么我们说S是一个完美匹配perfect matching

二分图：把图中的点分为 A，B 两组，使得左右的边都跨界组的边界。也就是说，所有的边都是一端在 A 中，另一端在 B 中。

匹配：S 是一个边的集合，任意两条边都没有公共的顶点。


![](http://img.renfei.org/2013/08/1.png)
![](http://img.renfei.org/2013/08/2.png)
![](http://img.renfei.org/2013/08/3.png)
![](http://img.renfei.org/2013/08/4.png )
上图4中的红线，即为一个matching的集合 S




# Reducing Bipartite Matching to Net Flow
如下图所示，把所有的边都给定一个方向，从 A 指向 B ，并增加 $s$ 和 $t$ 两个节点：

![](http://4.bp.blogspot.com/-nkBJG5elg8Y/UDiwHaW4JwI/AAAAAAAAOH0/8jjEFwWudHQ/s800/reduction-to-maxflow.png)

我们假设每条边的流量都为1，那么就可以用 Ford-Fulkerson algorithm 把它当做最大流问题解决了。

**Notes**
1. 因为 capacity 是整数，我们的 flow 也是整数
2. 因为 capacity 都是 1 ，我们要么走这条边，要么不走
3. 设集合 M 是我们经过的的边的集合
    -  M 就是一个匹配的集合
    - 而且 M 是最大匹配

对于 A 中的每一个顶点，我们只能挑一条边从这个点流出（通过 $s$ 到 A 中的每个点的 capacity 都是 1 解决）
对于 B 中的每一个顶点，我们也只能挑一条边流进这个点（通过 B 中的每个点到 $t$ 的 capacity 都是 1 解决）

如果最大流量是 $f$ ，那么最大匹配数也是 $f$

# Running Time
1. The running time of Ford-Fulkerson is $O(m'C)$，
2. $m'$ is the number of edges, $m'$是加了$s$和$t$以后的新的图$G'$边的数量
3. and $C = \sum_{e \text{ leaving } s} C_e$.
4. $C=|A|=n$，$C$也就是A中顶点的数量
5. The number of edges in $G'$ is equal to number of edges in $G$ plus $2n$. 
5. So,running time is $O((m+2n)n)=(mn+n^2)=O(mn)$

***
over！