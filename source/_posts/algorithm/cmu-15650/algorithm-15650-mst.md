---
title: CMU 15650 - Minimum Spanning Tree
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-02 21:22:45
---
CMU 15650 - Algorithm and Data Structure 

Minimum Spanning Tree
<!-- more -->

***

## Graph
Graph: graphs specify pairwise relationship between objects.
图展示了对象与对象之间的关系

undirected tree: $G = (V, E)$
`V` is a set of nodes (verticles) 顶点的集合
`E` is a set of edges. 边的集合

## Trees
Trees: A graph $G$ is a tree if it is connected and contains no cycles.
如果一个图中的点都相连而且这个途中不存在环，那么这个图就是一颗树

1. $T$ is a tree 
2. $T$ contains no cycles and $n-1$ edges 树有$n-1$条边
3. $T$ is connected and revoming any edge will disconnect it. 
    从一棵树里移除任何一条边都会将这个树断开，因为没有环嘛。
4. Any two nodes in a tree has only one path.
    树中的任意两个顶点间只有一条唯一的路径。
5. $T$ is acyclic and adding any new edge creates exactly one cycle.
    在树种添加任意一条边都会形成一个环。

## The Minimum Spanning Tree
Given a undirected graph G with all non-negative weights.
Find a Subgraph $T$ that connects all verticles and minimizes cost(T).
给定一个无向图，图中的每一条边都有一个非负的权重，要求在图中找到一个子图，这个子图连接了图中所有的点，并且要使得这个子图中的边的权重和最小。

$$cost(T)= \sum_{ (u,v) \in T}d(u, v)$$

T will be a tree:
If there is a cycle, we could remove any of the edge on the cycle to get a new subgraph `T'` with smaller cost(`T'`)
得到的这个子图肯定是一棵树，否则的话，我们可以移除任意一条环中的边，这样图中的所有顶点还是相连的。

## MST Property Summary
### MST Cut Property
**Theorem: **
The minimum weight edge $e = (v, w)$ between |S| and |V|-|S| must be in every the MST
切分定理：把一个图的顶点分为两个部分|S|，|V|-|S|，那么在所有连接这两个部分的边里面，最短的那条边肯定属于MST。


**proof:**
If $T$ doesn't contain $e$, Because $T$ is connected, there must be path $P$ between $v$ and $w$. Assume $P$ contains $f$ that "cross the cut" between |S| and |V|-|S|.
The subgraph $ T' = T - f \cup e$ has lower weight than $T$. 
证明：如果最短边$e$不在MST中，去掉连接这两个部分的边$f$，再把$e$加上去总是能使这棵树的总权重更小

### MST Cycle Property
**Theorem:**
Let $C$ be a cycle in $G$. Let $e = (u, v)$ be the edge with maximum weight on $C$. Then $e$ is not in any MST of G.
环定理：如果图中有一个环，那么这个环中的最长边一定不是MST中的一条边

**Proof:**
Suppose the theorem is false. Let $T$ be a MST that contains $e$
Deleting $e$ from $T$, partioning $T$ in to two sets: $S$(contains $u$ ) and $V-S$ (contains $v$).
Cycle $C$ must have some other edge $f$ that goes from $|S|$ and $|V-S|$.
Replcing $e$ by $f$ produces a lower cost tree, contradicting that T is an MST.
证明：假设环中的最长边$e$在MST中，$|S|$和$|V-S|$由$e$相连，我们去掉$e$，总能找到比$e$更短的另一条边，重新把这棵树连起来，这样总权重就减小了。


## Prim's MST algorithm
**Algorithm:**
Starting with any root node, add the $frontier edge$ with the smallest weight.
Prim算法：每次都添加最短邻边。

**Assumption:**
no two edge in G have the same edge cost.
If it doesn't true, we can add a very small value $\epsilon_e$ to the weight of every edge $e$

**Correctness:**
* Theorem: Prim's algorithm produces a minimum spanning tree
* Proof:
	At any point, $T = (V_T, E_T)$ is a subgraph that is a Tree.
	$T$ grows by 1 vertex and 1 edge after each iteation. So it will stop after $|V_G| - 1$ iterations and at that point $T$ will be a spanning tree.
	The pair $(V_T, V_G - V_T)$ is a cut of $G$
	By the cut property, the MST con

    tains the lowest cost edge crossing the cut. And this is the edge Prim's algorithm adds to T. So Prim's only add edges that are in MST
    切分定理可以证明

## Reverse-Delete Algorithm
Algorithm:
Remov edges in decreasing order of weight, skipping those whose removal removal will disconnect the graph. (largest edge in an cycle)
每次都移除最长边，跳过那些会disconnect the graph的边

Guarantee that the graph is not disconnected, deleting edge with maximum weight $e$ and there must be *another path* between u and v

## Kruskal's Algorithm
### Algorithm
**Algorithm:** 
add edges in increasing weight, skipping those whose addition could create a cycle.
Kruskal算法，将边按从小到大的顺序添加到图里，跳过那些会形成环的边。

**Proof**:
(Cut Property)
$S$ = nodes which $v$ has a path just before $e$ is added
$u$ is in $V-S$ (otherwise there would be a cycle)
仍然可以用切分定理证明

### Data Structure for Kruskal Algorithm:

接下来要解决的是用什么数据结构来构造Kruskal算法
**How to check if adding an edge $(u, v)$ would create a cycle?**
- If $u, v$ are already in the same component.
- We start with a component for each node.
- Components merge when we add an edge.
- Need a way to check $u$ and $v$ are in same component and to merge two components into one.

### Array based Union-Find
先来介绍以数组为基础的Union-Find数据结构。

#### Operations
- MakeUnionFind: create the data structure containing |S| sets, each containing one item. O(|S|)
初始化，在一开始的时候才会用到。初始化简历|S|个集合，也就是每个顶点都自成一格集合。
- Find(i): return the name of the set containing item $i$. O(1)
查找操作，找到某个顶点的所属的集合的名字
- Union(a,b): merge two sets **with name $a$ and $b$** into single set 
    prepend smaller list to larger list. Set size[y] = size[y] + size[x]
    合并操作，把两个集合和并起来。总是把小的集合接到大的集合后面。

####  Data Structure
要用到两个数组和一个list的数组
- UF Sizes (array) 一个数组来存每个集合的大小
- UF Sets (array) 一个数组来存每个集合的名字
- UF items (array of list) 每个集合都有一个元素列表，这些所有的列表组成一个集合

![Data structure](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-mst/union-find.png "Data structure")


#### Runtime of Union Find (array based):
**Theorem:**
Any sequence of $k$ union operationon a collection of $n$ items takes time at most proportion to $klog(k)$.
$k$次union操作的时间复杂度是$klog(k)$

**Proof:**
After $k$ unions, at most $2k$ items have been involved in a union. (each union can touch at most 2 **new items**)

1. Every time $set(v)$ changes, the size of the set at least doubles.
2. $Set[v]$ can have changed at most $log_{2}(2k)times$

At most 2k items have been modified at all, and each was updated at most $ log_{2}(2k) $.

So totally $ 2klog_{2}(2k) $

证明：
1. 在$k$次union操作以后，最多涉及$2k$个元素，因为每次的union最多涉及两个$新的元素$。
2. 每次一个顶点的所属的集合改变的时候，也就是被合并的时候，合并后的集合的大小至少比原来的集合大一倍，因为我们在合并两个集合的时候，改变元素数量小的集合的名字，保留元素数量大的集合的名字。

综上所述，在$k$次union操作以后，一个集合里最多有$2k$个元素，集合中的每个元素所属的集合最多可能改变了$log_{2}(2k)$次。

所以总的时间复杂度是$2klog_{2}(2k) = klog(k)$

### Running time of Kruskal using Array based UF
$m$是边的数量
1. Sorting the edges: $O(mlogm)$
$m \leqslant n^2$, so $log m < log{n^2} = 2log n$ 
排序的时间复杂度是$O(mlogm) = O(mlogn)$
2. At most $2m$ "find" operations: $O(2m)$ (To check if u and v are in the same component)
每次要添加一条边的的时候，都要检查两个顶点是不是在同一个集合中，不是的话才能加进来。所以要检查$O(2m)$次
3. At most $n-1$ union operations: $O(nlogn)$ 

Total : $mlogn + 2m + nlogn = O(mlogn)$

### Tree based Union Find

![Tree Based Union Find](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-mst/tree-based-union-find.png "Tree Based Union Find")

**Operations:**
- MakeUnionFind: $O(n)$
- Find(i): $O(logn)$
- Union: $O(1)$

**Theorem:**
$Find(i)$ takes $logn$ time in tree based union find data structure with $n$ items.
查找操作的时间复杂度变为了$logn$

**Proof:**
The depth of an item equals the number of times the set is renamed.
When renamed, the size of the set at least doubles.
the largest number of times the set renamed is $log_{2}n$
用树结构的话，查找的时间复杂度与这个元素在树中的深度相同。
每次合并改变一个集合的名字的时候，同样，一个集合中的元素的数量翻倍。
因为顶点的总数就是$n$，所以深度最多也就是$log_2 n$

### Running time of Kruskal using Tree based UF
Same with Array based:
1. Sorting: $O(mlogn)$
2. Find: $2mlog(n)$
3. Union: $n-1$

Total: $O(mlogn)$

## Implementing ClostVertex (in Prim's)
### Priority Queue ADT & Heaps
Prim算法使用堆来实现的
(key(v), v)

**operations:**
1. H.insert(i)
2. H.deleteMin()
3. H.findMin()
4. H.delete(i)
5. H.makeHeap(S)

Items: vertices that are on the "frontier"
key(v) = distToT[v]
ClostVertex: return H.deleteMin()

### Run time
findMin: return root. $O(1)$
insert: add as leaf, heapify-up. $O(logn)$
delete(i): swap(i, leafNode), delete i, heapify-down the leafNode. $O(logn)$

Store heap in a **Complete Tree** (Array)
完全树，也就是只要叶子节点那一层可以不满，这个可以用Array来实现。

### One more operation in Prim: 
H.decreaseKey($u$, $j$); reduce the key for item $u$ by $j$ 
Reduce the key --> heapify up. $O(logn)$

### Make Heap 
1. put items in array arbitrarily
2. for $i = n ... 1$, heapify down(i)

Total time = 
$$ \sum_{h}h(n / 2^{h}) = n \sum h/{2^h} = O(n)  $$


## Clustering
An appllication of MST

**Goal:**
1. Divide $n$ items into $k$ groups
2. so that the **minimum distance** between items in different groups is maxmized.

**Idea:**
Kruskal's and stop when there are k clusters

**Correctness:**
Another  view:
1. delete the k-1 most expensive edges from the **MST** (From MST not the graph)
2. the spacing $d$ of the clustering $C$ that this produces is the length of the $(k - 1)^st$ most expensive edge.

Let $C'$ be a different clustering.

Since $C  \neq C'$, there must be some pair $p_i$, $p_j$ that are in same cluster in $C$, but different clusters in $C'$.

Together in C, in the cluster containing $p_i$, $p_j$, all edges $\leqslant d$.
But some path in the cluster passes beteen two differnt clusters in $C'$

Therefore, separationo of  $C' \leqslant d$. So $C$ is the right clustering.
 








