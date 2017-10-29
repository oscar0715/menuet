---
title: CMU 15650 - Dijkstra
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-04 23:21:45
---
CMU 15650 - Algorithm and Data Structure 

Dijkstra Algorithm
<!-- more -->

***

# Shortest path 

**Shortest Paths:**
Given directed graph $G$ with $n$ nodes, and non-negative lengths on each edge, find the $n$ shortest paths from a given node $s$ to each $v_i$.（Every $v_i$ has its own shoertest path from $s$ to it.）

给定一个图 $G$ 和一个起点 $s$ ，找到从起点 $s$ 到余下所有其他点的最短路径。

***

# nextEdge for Shortest Path
We discussed tree growing in the Graph Traversal 
树的增长的算法
{% codeblock lang:java  %}
TreeGrowing(graph G, vertex v, func nextEdge): 
    T = (v,∅)
    S = set of edges incident to v
    while S is not empty:
        e = nextEdge(G, S)
        T = T + e                // add edge e to T
        S = updateFrontier(G, S, e)
    return T
{% endcodeblock %}

Dijkstra’s algorithm is just a special case of tree growing.

**nextEdge for Shortest Path （函数nextEdge）:**
- Let $u$ be some node that we have already visited (it will be in S).
    假设点$u$是我们已经访问过的一个点。我们把这些访问过的点的集合设为$S$
- Let $d(u)$ be the length of the $s−u$ path found for node $u∈S$.
    我们记 $d(u)$ 为从起点 $s$ 到点 $u$ 的路径的距离
- nextEdge: return the frontier edge $(u,v)$ for which $d(u)+length(u,v)$ is minimized.
    nextEdge这个函数呢，就是从集合 $s$ 中的点的邻点中找到一个点 $u$ ，使得 $d(u)+length(u,v)$ 最小
- The “$d(u)$” term is the difference from Prim’s algorithm.

***

# Dijkstra’s Algorithm (in KT book)
## Algorithm
![Dijkstra](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-dijkstra/dijkstra.jpeg "Dijkstra")

while loop: $u$ 是 heap 中的一个点

For loop: 遍历 $u$ 的所有邻点 $v$，这个$v$ 可能是之前遍历到过的，因为 $v$ 可能同时是不同 $u$ 的邻点. For loop 结束以后要把 $u$ 从堆中删去，因为它的历史使命完成了，以它为跳板的路线的长度都已经确定了。

## Proof of Correctness
**Theorem.** 
Let T be the set of nodes explored at some point during the algorithm. For each $u ∈ T$ , the path to $u$ found by Dijkstra’s algorithm is the shortest.
对于已经用 Dijkstra 算法访问过的任意点 $u$ ，从起点 $s$ 到点 $u$ 的距离都是最短的。

**Proof:** 
By induction on the size of $T$ . 用数学归纳法来证明
1. Base case: When $|T| = 1$, the only node in $T$ is $s$, for which we’ve obviously found the shortest path.
    第一步，如果整个图只有一个起点 $s$ ，那么显然我们可以得到最短路。
2. Induction Hypothesis: Assume theorem is true when $|T| ≤ k$. Let $v$ be the $(k+1)^{st}$ node added using edge$(u,v)$.
    归纳的假设：假设我们的定理在前 $k$ 个点都是对的，我们记 $v$ 是第 $(k+1)$ 个点
3. Let $P_v$ be the path chosen by Dijkstra’s to $v$ and let $P$ be any other path from $s$ to $v$.
    我们记 $P_v$ 为用 dijkstra 算法找到的路径， $P$ 是任意一条别的路径

![proof](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-dijkstra/proof.jpeg "proof")

The path to $v$ chosen by Dijkstra’s is of length ≤ the alternative blue path.
PS: Dijkstra’s choose $v$ as the next node so $d(y)$ must larger than $d(v)$

如上图，根据 dijkstra 算法， $d(y)$ 都比 $d(v)$ 要小了，不用说 $d(y)+length(y,v)$ 了

## Implementation of Dijkstra
![Implementation of Dijkstra](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-dijkstra/dijkstra-alg.jpeg "Implementation of Dijkstra")



## Runtime of Dijkstra's Algorithm

Same as Prim’s MST algorithm:
1. Every edge is processed in the for loop at most once. 
    This is because the graph is directed
    While 循环会遍历到每一个点的每一条边，因为边是有向的，我们只挑从 $S$ 出发的边，所以是每条边一次而不是两次
2. In response to that processing, we may either 
    遍历到这条边的时候，有下面这些选择：
    (1. do nothing, $O(1)$; —— 不满足更新条件
    (2. insert a item into the heap of at most $|V|$ items, $O(log|V|)$; —— 这是一个新的点，加到 heap 中
    (3. reduce the key of an item in a heap of at most $|V|$ items, $O(log|V|)$; —— 点已经在 heap 中了，但是有更短的路径，所以我们要更新，然后 reduce key

Total time is therefore: $O(|E|log|V|)$.