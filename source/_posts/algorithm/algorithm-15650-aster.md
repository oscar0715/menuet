---
title: CMU 15650 - A *
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-05 21:21:45
---
CMU 15650 - Algorithm and Data Structure 

A star Algorithm
<!-- more -->

***

**Using a heuristic for shortest paths**

Dijkstra’s algorithm assumes it knows nothing about nodes it hasn’t reached during the algorithm.
Dijkstra算法假设对没有访问过的点是一无所知的。

Suppose instead we have **$h(u)$** which is an estimate of the distance from node $u$ to $t$.
现在我们假设对未访问过的点也有一定的认知，记 $h(u)$ 是点 $u$ 到 $t$ 的距离。 $t$ 是终点。

# A* algorithm
Maintain two values for every visited node:
- $g(u)$ = best distance from $s$ to $u$ found so far.
- $f(u)=g(u)+h(u)$ = estimate of the length of the best path from $s$ to $t$ through $u$.
- $g(u)$ 是 $s$ 到 $u$ 的距离，$h(u)$ 是 $u$ 到 $t$ 的距离，加起来就是 $f(u)$ ，也就是经过点 $u$ 的 $s$ 到 $t$ 的最短距离。

Idea: Run a Dijkstra-like algorithm using  $f(u)$  as the key
Dijkstra’s algorithm: $f′(u) = d(v) + length(v,u)$ 
其实 Dijkstra 算法只用了 $g(u)$ , 而 $h(u) = 0$

# Implementation
{% codeblock lang:java %}
g[s] ← 0
f[s] ← g[s] + h[s]
Heap ← MakeHeap((s,f[s]))  // heap key is f[s] repeat
u ← DeleteMin(Heap)        // Expand node with minimum f[u] if u = goal then return
for v ∈ Neighbors(u) do
    if g[u] + d(u,v) < g[v] then
        parent[v] ← u
        g[v] ← g[u] + d(u,v)  // 更新真实的距离 g(v)
        f[v] ← g[v] + h(v)    // 更新预计的经过 v 的 从 s 到 t 的距离

        if v not in Heap then
            Insert(Heap, v, f[v]) 
        else
            ReduceKey(Heap, v, f[v])  // Sift up for new key 
until Heap is empty
{% endcodeblock %}

# Choice of $h(u)$
**Definition (Admissible):**
Let $h^\*(u)$ be the real shortest distance from $u$ to $t$.  
$h^\*(u)$ 是从点 $u$ 到 $t$ 的真实的最短距离

A heuristic $h(u)$ is admissible if $h(u) ≤ h^\*(u)$ for all $u$.
在算法计算的过程中，只有当 $h(u) ≤ h^\*(u)$ 时，算法才是可接受的

When $h(u) = 0$ for all $u$: A* is equivalent to Dijkstra’s algorithm.
This choice of $h(u)$ is obviously admissible.
Dijkstra 算法取 $h(u) = 0$ 

**Theorem.** 
If $h(u)$ is admissible, then A* is guaranteed to find an optimal route to the destination $t$.

**Proof:**
此处的证明没看懂Orz
算了，就这样吧，没时间看了，费脑子………………



