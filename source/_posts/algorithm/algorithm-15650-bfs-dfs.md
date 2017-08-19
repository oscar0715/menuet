---
title: CMU 15650 - Graph Traversals
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-04 12:04:45
---
CMU 15650 - Algorithm and Data Structure 

Graph Traversals 
<!-- more -->

***

# DFS
## Depth-First Search
DFS keeps walking down a path until it is forced to backtrack.
It backtracks until it finds a new path to go down.
深度优先搜索先沿着一条路一直往下走，实在走不下去了，再回头原路返回，知道找到一条可以重新继续往前走的路。

It results in a search tree, called the depth-first search tree.
In general, the DFS tree will be very different than the BFS tree.
深度优先搜索算法都到的深度优先搜索树，通常来说 DFS 树和 BFS 树是不一样的。

## A property of Non-DFS-Tree Edges
**Theorem 定理:** 
Let x and y be nodes in the DFS tree $T_G$ such that $\lbrace x,y \rbrace$ is an edge in undirected graph $G$. Then one of $x$ or $y$ is an ancestor of the other in $T_G$ .
如果 $x$ ， $y$ 是 DFS 树中的两个节点，这两个节点之间有一条无向边，那么两个节点其中的一个一定是另一个的祖先节点。

**Proof 证明:** 
Suppose, $x$ is reached first in the DFS. When we reach $x$, node $y$ must not yet have been explored.
假设，在DFS中 $x$ 先被遍历到了，这个时候 $y$ 还没有遍历到。

All the nodes that are marked explored between first encountering $x$ and leaving $x$ for the last time are  of $x$ in $T_G$ .
接着，我们第一次从 $x$ 出发，沿着这条路径往下走，直到然后最后一次回到 $x$ ，并离开 $x$ 。这个过程中间的所有的点都是 $x$ 的descendants，这里叫成后代真的也是有点奇怪。

$y$ must become explored before leaving $x$ for the last time (otherwise, we should add $\lbrace x,y \rbrace$ to $T_G$). Hence, $y$ is a descendent of $x$ in $T_G$.
因为 $x$ ， $y$ 是有一条边连着的，所以 $y$ 肯定也会在上文提到的那个过程中被遍历到，所以 $y$ 是 $x$ 的后代。

# BFS
## Breadth-First Search
Breadth-first search explores the nodes of a graph in increasing distance away from some starting vertex $s$.
深度优先搜索是按照一步一步远离根节点的顺序来遍历。

It decomposes the component into **layers** $L_i$ such that the shortest path from $s$ to each of nodes in $L_i$ is of length $i$.
BFS 把整张图拆分成许多*层*，于是从根节点 $s$ 到第 $L_i$ 层的每一个节点的距离都是 $i$.

Breadth-First Search:
1. $L_0$ is the set $\lbrace s \rbrace$.
第 $L_0$ 层只有一个根节点。
2. Given layers $L0$, $L1$ ,..., $Lj$, then $L_{j+1}$ is the set of nodes that are not in a previous layer and that have an edge to some node in layer $L_j$.
{% raw %}第 $L_{j+1}$ 层中的节点一定不在之前的层中，而且和第 $L_j$ 层的节点有边相连。{% endraw %}

## Property of Non-BFS-Tree Edges
不在BFS树中的边的性质

**Theorem：**
Choose $x \in L_i$ and $y \in L_j$ such that $\lbrace x,y \rbrace $ is an edge in undirected graph $G$. Then $i$ and $j$ differ by at most 1.
如果 $x \in L_i$ ，  $y \in L_j$ ，且这两个点之间有一条边 $\lbrace x,y \rbrace $ 相连，那么这两个点之间的层数只相差1。

In other words, edges of $G$ that do not appear in the tree connect nodes either in the same layer or adjacent layer.
也就是说，在图中的任意一条边的两个端点，要么在同一层，要么在隔壁层。

**Proof:**
Suppose not, and that $i < j - 1$
All the neighbor of $x$ will be found by layer $i + 1$
Therefore, the layer is less than $i + 1$, so $j \leqslant i + 1$, which contradicts $i < j - 1$
我们假设 $i < j - 1$，$x \in L_i$ ，也就是 $i$ 比 $j$ 小了不止一层。
但是因为所有 $x$ 的邻点都会在下一层，也就是 $i + 1$ 层中被遍历到，所以 $j \leqslant i + 1$ ，这与假设矛盾了。


# Tree Growing
## Process
We can think of BFS and DFS (and several other algorithms) as special cases of tree growing:
1. Let T be the current tree, and
2. Maintain a list of frontier edges: the set of edges of G that
have one endpoint in T and one endpoint not in T:
3. Repeatedly choose a frontier edge (somehow) and add it to T.

我们可以把DFS和DFS看成一种特殊的树的成长方式，两者都是按照以下步骤走的：
1. $T$ 是图 $G$中一颗树
2. 一部分点在 $T$ 中，另外一部分不在 $T$ 。这个时候我们有一个 frontier edges 的集合，这些边的一端在 $T$ 中，另外一端在另外一部分中
3. 我们只需要不断地从这歌儿集合中挑选一条边到集合中就好了。

## pseudocode
``` Java
TreeGrowing(graph G, vertex v, func nextEdge): 
	T = (v,∅)
	S = set of edges incident to v  // frontier edges
	while S is not empty:
		e = nextEdge(G, S)       // 挑选出下一条要加入T的边
		T = T + e                // add edge e to T
		S = updateFrontier(G, S, e) // 更新S这个集合
return T

```

1. The function $nextEdge(G, S)$ returns a frontier edge from $S$.
	nextEdge这个函数负责挑出一条边来，这条边就是要加到 $T$ 中的
2. $updateFrontier(G, S, e)$ returns the new frontier after we add edge e to $T$.
	因为有新的边加入了，所以需要 $updateFrontier$ 这个函数来更新 frontier edges 这个集合。
3. So For $BFS$, $S$ is a queue, and $nextEdge$ poll the ealiest edge from queue, updateFrontier add neighbors to the end of the queue
	对于 $BFS$ 来说，$S$ 是一个队列，这个队列存着所有的邻边，$nextEdge$ 按照先进先出的顺序，选择最先遇到的边加入树。
4. For $DFS$, $S$ is a stack, and nextEdge pop the latest edge from the stack. updateFrontier push the neighbors to the stack.
	对于 $DFS$ 来说， $S$ 是一个栈， $nextEdge$ 按照后进先出的顺序，每次都把最后找到的边作为加入 $T$ 的边。

## Tree Growing Algorithms
These algorithms are all special cases / variants of Tree Growing, with different versions of nextEdge:
1. Depth-first search
2. Breadth-first search
3. Prim’s minimum spanning tree algorithm 
4. Dijkstra’s shortest path
5. A*

## nextEdge
1. What’s nextEdge for DFS?
	- Select a frontier edge whose tree endpoint was discovered **most recently**.
	- We can use a **stack** to implement DFS.
	- Runtime for DFS: $O(|Edges|)$
2. What’s nextEdge for BFS?
	- Select a frontier edge whose tree endpoint was discovered **least recently**.
	- We can use a **queue** to implement DFS.
	- Runtime for BFS: $O(|Edges|)$

 $nextEdge$ 在 $DFS$ 和 $BFS$ 中的作用，在上文一节介绍过啦。


# Representing Graphs
For a graph $G = (V, E)$
$n =|V|$ is the number of nodes, $m = |E|$ is the number of edges.

图的字母表示。

## Two presentations
There are two presentations of a graph. 两种存储图的数据结构
1. Adjacency matrix. 邻接矩阵
	- takes $\Theta (n^2)$ space, but $G$ may have much fewer edges than $n^2$
		邻接矩阵要花费 $\Theta (n^2)$ 的时间复杂度，但是如果其实没有这么多边的话，很多空间都浪费了。
	- checking incedent edges takes $\Theta (n)$ time, but a node may have much fewer nodes.
  	- So Adjcency Maxtrix is inefficient.
  		所以邻接矩阵效率很低的
2. Adjcency list. 邻接表
	- takes $O(m + n)$ space
	- works better for sparse graphs.
		对于稀疏图来说，邻接表的性能更好。

## Adjcency list
We will use adjcency list! 
那我们就来看看邻接表的实现：
  - $Adj[v]$ records all **adjcent node** to $v$
  	$Adj[v]$这个数组，保存了所有和$v$相邻的点。
  - each edge $e = (v, w) \in E$ occurs on two adjcency lists.
  	

It takes $O(m + n)$ space:
1. We need an array of pointers to set up lists in Adj (length = n, $O(n)$)
初始化的时候，我们需要建立一个长度为n的指针数组来实现这个邻接表，也就是说，每一个顶点都有自己的一个邻点的集合。
2. We need space for all the lists, totally takes $2m = O(m)$
所有list的空间加起来是$2m$

Another way to justify this bound:
1. Define $degree(n_v)$ of node $v$, which is the number of its *incident edges*
2. The length of the list at $Adj[v]$ is is $n_v$
3. So the totall length over all nodes is $O(\sum_{v \in V} n_v) = 2m$

**Proof:**
Each edge $e = (v,w)$ contributes exactly twice to this sum: once in the quantity $n_v$ and once in the quantity in $n_w$. Since the sum is total of the contribution of each edge, it is $2m$.

定义顶点 $v$  的度 $degree(n_v)$ ，也就是每个顶点的有多少个相邻的顶点，或者说多少条从这个顶点出发的边（这里是无向图）。所有个顶点的度加起来的和是 $2m$ ，因为每条边有两个顶点，被算了两次。

The adjcent list is a natural representation for exploring graphs. If the algorithm is currently looking at a node $u$, it can read this list of neighbors in constant per neighbor.

Thus, list representation correspinds to a physical notion of "exploring" the graph. You can learn the neighbors of a node $u$, once you arrive at $u$.

# Implementations of BFS
## BFS implementation
``` Java
procedure bfs(G, s):
	Q := queue containing only s  // 初始化的时候这个队列只包含根节点，也就是出发点
	while Q not empty
		v := Q.front();           // 取队列的第一个顶点
		Q.remove front()          // 并顺便从队列中移除
		for w ∈ G.neighbors(v):   // 遍历这个顶点的所有相邻的顶点
			if w not seen:        // 如果这个顶点还没遍历到过，那么就加到队列中
				mark w seen
				Q.enqueue(w)
```

## n-Layers Implementation
``` Java
BFS(s):
Set Discovered[s] = true and Discovered[v] = false for all other v 
Initialize L[0] to consist of the single element s
Set the layer counter i = 0
Set the current BFS tree T = ∅
While L[i] is not empty
	Initialize an empty list L[i + 1] 
	For each node u ∈ L[i]
		Consider each edge (u, v) incident to u 
		If Discovered[v] = false then
			Set Discovered[v] = true
			Add edge (u, v) to the tree T
			Add v to the list L[i+1] 
		Endif
	Endfor
	Increment the layer counter i by one 
Endwhile
```

## Runtime Analysis 
If the graph is *adjcent list representation*, the implementation of BFS runs in time $O(m+n)$.

**Proof:**
1. There are $n$ lists $L[i]$ that need to set up, it takes $O(n)$.
2. Each node occurs on at most one list, so **For** loop runs at most n times over all iterations of the **While** loop.
3. The time spent in the For loop, considering edges incident to node $u$ is $O(n_u)$
4. So the total time over all nodes is  $O(\sum_{v \in V} n_v) = 2m$ (Same to the space analysis)
5. We need $O(n)$ additional time to set up the array **Discovered**
6. Totally $O(m+n)$

时间复杂度和空间复杂度的算法是一样的。

# Implementations of DFS
## Recursive implementation of DFS
{% codeblock lang:java  %}
procedure dfs(G, u):
	while u has an unvisited neighbor in G
	   v := an unvisited neighbor of u
	   mark v visited
	   dfs(G, v)
{% endcodeblock %}

## Stack-based implementation of DFS
{% codeblock lang:java  %}
procedure dfs(G, s):
	S := stack containing only s
	while S not empty 
		v := S.pop()
		if v not visited: 
			mark v visited
			for w ∈ G.neighbors(v): S.push(w)
{% endcodeblock %}

## Runtime Analysis 
The number of nodes which will be added into $S$ is still $2m$, so it also takes $O(m+n)$ time.

# An application of BFS
##  Bipartiteness
A graph is bipartite if you can divide its nodes into 2 parts $(S,|V|−S)$ (a cut) so that every edge goes between the two parts $S$ and $|V| − S$ and not within a single part.

二分图：如果能把一个图中的顶点分成两部分，所有的边都是横跨这两部分的，那么这个图就是一个二分图

![二分图](https://upload.wikimedia.org/wikipedia/commons/e/e8/Simple-bipartite-graph.svg "二分图") 

Problem: Determine if a graph $G$ is bipartite. 
我们要解决的问题是怎么判断一个图是不是二分图

Bipartite graphs can’t contain odd cycles
一个重要的定理是二分图不能有奇数环，也就是环中的顶点数量不能使奇数。

## Bipartite Testing
**solution:**
1. Do a BFS starting from some node s.
2. Color even levels “blue” and odd levels “red.”
3. Check each edge to see if any edge has both endpoints the same level.

对一个图从 $s$ 出发，进行 $BFS$ ，把偶数层的顶点涂成蓝色，奇数层的顶点涂成红色。检查每一条边的两个顶点的颜色是不是都不一样。

**Proof:**
One of two cases happen:
1. There is no edge of $G$ between two nodes of the same layer. In this case, every edge just connects two nodes in adjacent layers. But adjacent layers are oppositely colored, so $G$ must be bipartite.
如果每一条边的两个顶点的颜色都不一样，也就是说可以把所有顶点恰好分成两组，红色一组，蓝色一组，所有边都横跨在这两组之间。

2. There is an edge of $G$ joining two nodes $x$ and $y$ of the same layer $L_j$ . Let $z ∈ L_i$ be the least common ancestor of $x$ and $y$ in the BFS tree T.
$z −x −y −z$ is a cycle of length $2(j−i)+1$, which is odd, so G is not bipartite. 
$(z-x)$ is $(j-1)$, $(z-y)$ is $(j-1)$, $(x-y)$ is 1, so totally $2(j-1) + 1$ 
如果有一条边的两个顶点的颜色是一样的。我们先找到两个顶点最近的那个共同的祖先 $z$ ,由下图可知，这个环的长度是 $2(j-1) + 1$ ，是奇数。所以说一个图里有奇数环，就能证明这个图不是二分图。

![Proof](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-bfs-dfs/Bipartite.png "Proof")

# An Application of DFS
## DAGs
有向无环图：
A directed, acyclic graph (DAG) is a graph that contains no directed cycles. (After leaving any node u you can never get back to u by following edges along the arrows.)

**Theorem:**
Every DAG contains a vertex with no incoming edges.
定理：每一个有向无环图都有一个顶点是没有入边。

**Proof:** 
Suppose not.
Then keep following edges backward and in fewer than n + 1 steps you’ll reach a node you’ve already visited.
This is a directed cycle, contradicting that the graph is a DAG.
如果一个有向无环图是有入边的。我们随便找一个边，沿着有向边向反方向转，迟早会访问同一个节点访问两次。总之所有顶点都有入边就意味着有环。

## Topological Sort
**Topological Sort 拓扑排序:**
Given a DAG $D = (V , E)$, find a mapping $f$ from $V$ to {1,...,$|V|$}), so that for every edge $(u,v) ∈ E$, $f(u) < f(v)$, and each integer $1 ≤ i ≤ |V|$ is mapped to exactly once.
给定一个有向无环图 $D = (V , E)$，对图中的每一个顶点的赋一个值$f$，对于每一条图中的边$(u,v) ∈ E$ 来说，都有 $f(u) < f(v)$ ，注意这里是有向的。$f$ 的值 $i$ 的范围：$1 ≤ i ≤ |V|$，每个整数都唯一配对一个顶点。

## Topological Sort Algorithm
Topological sort:
1. Let i = 1
2. Find a node u with no incoming edges, and let f (u) = i 
3. Delete u from the graph
4. Increment i

方法就是每次都找到一个没有入边的顶点，赋值i，把这个顶点从图中删去，i++。

Implementation: Maintain
1. Income[w] = number of incoming edges for node w
2. a list S of nodes that currently have no incoming edges.

实现的时候建立一个数组 $Income[w]$，来保存顶点 $w$ 还有多少入边。还需要一个list $S$，来保存现在有哪些顶点没有入边。

## Topological Sort Running Time
1. Initialize $Income[w]$ array:
  - Count the number of edges adjacent to each node.
  - Implement via BFS or DFS.
  - Total running time $O(|V| + |E|)$.
  - 用BFS或者DFS来确定每一个顶点都有几条边
2. For each node that currently has no incoming edges, decrement an entry in the income array, and possibly add something to S
  - This executes $O(|V|)$ times.
  - Handling each neighbor (decrement, add to S) takes O(1) time.
  - So this entire step takes $O(|V| + |E|)$ time.
  - 每次移除一个顶点以后，都要更新Income[]这个数组

Total running time is: $O(|V|+|E|)+O(|V|+|E|) = O(|V|+|E|)$. This is $O(|E|)$ for connected graphs.

## Discovery and Finishing Times
DFS can be used to associate 2 numbers with each node of a graph G:
1. discovery time: $d[u]$ = the time at which u is first visited   
2. finishing time: $f[u]$ = the time at which all u and all its
neighbors have been visited. Clearly $d[u] \leqslant f [u]$.



















