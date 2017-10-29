---
title: CMU 15650 - Bellman-Ford
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-05 23:38:45
---
CMU 15650 - Algorithm and Data Structure 

Bellman-Ford Algorithm
<!-- more -->

***

## Shortest Path Problem

**Shortest Path with Negative Weights:**
Given directed graph $G$ with weighted edges $d(u,v)$ that may be positive or negative, find the shortest path from $s$ to $t$.
给定有向有权图，边的权重有正有负，找到 $s$ 到 $t$ 的最短路径

## Complication of Negative Weights
**Negative cycles:** 
If some cycle has a negative total cost, we can make the $s − t$ path as low cost as we want:
如果在图中存在一个总权重为负数的环，那么我们可以使包含这个环的路径的总长度到负无穷

Go from $s$ to some node on the cycle, and then travel around the cycle many times, eventually leaving to go to $t$.
因为我们可以一直沿着这个负环走啊走啊走啊

## Bellman-Ford
1. Let $dists(v)$ be the current estimated distance from $s$ to $v$. 
我们假设 $dists(v)$ 是从点 $s$ 到 $v$ 的估计的距离
2. At the start, $dists(s) = 0$ and $dists(v) = \infty$ for all other $v$.
起点的 dists(s) = 0$，其他点在初始情况下都是无穷大
3. Ford step. 
	Find an $edge(u,v)$ such that 
	$dists(u) + d(u,v) < dists(v)$
	and set $dists(v) = dists(u) + d(u,v)$.
4. Repeatedly Applying Ford Step until $dists(u)+d(u,v) ≥ dists(v)$

其实 Dijkstra 也是会做这一步。

**Theorem:**
After applying the Ford step until 

$$dists(u)+d(u,v) ≥ dists(v)$$

for all edges, $dists(u)$ will equal the shortest-path distance from $s$ to $u$ for all $u$.

只要不断重复的执行 Bellman-Ford Step，最后所有的 $dists(v)$ 都已经是最小的时候，就找到了最短路，因为不能更短了。

那么问题来了，我们怎么去找还可以做 Bellman-Ford Step 的边呢
**Which edges are candidates for the Ford rule?**
Ford rule:

$$dists(u) + d(u,v) < dists(v)$$

This can only become true if $dists(u)$ has gotten smaller since the
last time we checked.
如果 $dists(v)$ 需要被更新的话，原因肯定是它之前的那个节点 $u$ 的 $dists(u)$ 变小了
* Whenever we change $dists(u)$ we add u to a queue. 
	在改变 $dists(u)$ 的时候，我们就把 $u$ 加到一个 queue 里面，这就是做了一个标记。
* To look for an edge to apply the Ford rule to we take a node of the queue and look at all its edges.
	然后要做的就是从 queue 里面取出一点，然后去检查这个点的所有相邻的点就好了。

## Implementation
{% codeblock lang:java  %}
ShortestPath(G, s, t):
Initialize dist[u] = ∞ for all u
dist[s] = 0
// queue tracks nodes that are candidates for Ford rule queue = [s]
while queue is not empty:
	v = front of queue (and remove v)   // 从queue里面取出一个点
	for u ∈ neighbors(v):               // 对这个点的所有邻点都进行检查
		// Apply Ford rule if we can   
		if dist[v] + d(v,u) < dist[u]:  // 如果有必要，就做一个 Ford Step
			dist[u] = dist[v] + d(v,u) 
			parent[u] = v
			if u not in queue: put w at end of queue
{% endcodeblock %}

## Runtime
- n = number of nodes   
- m = number of edges

After $dists(v)$ has been updated $k$ times, it corresponds to a simple path of $k$ edges. （这个地方不是很理解……）（每做一次更新，都能确定一个新的节点的最短路）

A path can contain at most $n − 1$ edges (before nodes start to repeat), so each $dists(v)$ can be updated at most $n − 1$ times.

Updating all vertices once takes time $O(m)$ since we look at each edge twice.
Total running time = $O(mn)$. （m就是所有点的度的总和，这个好说）

Note that this is slower than Dijkstra’s algorithm in general.

Dijkstra 算法的本质是利用了贪心思想，是在剩下的节点中，找一个和 $S$ 最近的节点。
但是 Bellman Ford 算法不能这样做，因为有负边，所以只能去检查每一条边。

## Another view
动态规划来解决问题

{% codeblock lang:java  %}
ShortestPath(G, s, t):
	Initialize dist[u] = ∞ for all u 
	dist[s] = 0
	
	for i from 1 to |V|-1:
		# After round i, 
		# the i-th node of the shortest path must 
		# have its distance set to the correct value.
		for (v,u) in E:
			if dist[v] + d(v,u) < dist[u]:
				dist[u] = dist[v] + d(v,u)
				parent[u] = v
	
	for (v,u) in E:
		if dist[v] + d(v,u) < dist[u]:
		report that there is negative-weight cycle
{% endcodeblock %}

This is a Dynamic Programming:

**Definition:**
Let $dists(v,i)$ be minimum cost of a path from $s$ to $v$ that uses at most $i$ edges.

Can recursively define $dists(v,i)$:
1. If best $s−v$ path uses at most $i−1$ edges, then
$dists (v , i) = dists (v , i − 1)$.
2. If best $s−v$ uses $i$ edges, and the last edge is $(w,v)$, then $dists (v , i ) = d (w , v) + dists (w , i − 1)$.

**Recurrence:**
Let $N(w)$ be the neighbors of $w$.
$dists(v,i)$ = cost of best path from $s$ to $v$ using at most $i$ edges.

Recurrence:

$$
dists(v,i) = min
\left\\{
    \begin{matrix}
     	dists (v , i − 1) \\\\
     	min_{w∈N(v)} dist_s(w,i−1)+d(w,v) 
    \end{matrix} 
\right .
$$

Base case: $dist_s(v,1) = d(s,v)$ or ∞ if $(s,v)$ does not exist 

Goal: Compute dist_s $(t , n − 1)$.


