---
title: CMU 15650 - Final Review 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-12-14 14:09:45
---
CMU 15650 - Algorithm and Data Structure 

Final Review 
<!-- more -->

***

# Runtime

## Array-based Union-Find
k 次 Union-Find 花费 O(klogk)，所以每次是 O(logk)
1. 在 k 次 Union-Find 以后，最多有 2k 个元素
2. 对于其中的某个集合 set[v] 来说最多也是 2k 个元素，然后每次合并的数量至少也是 double
3. 所以这个集合变动的次数最多也就是 $log_2 (2k)$
4. 最多 2k 个元素被改动了，每个元素（让它代表一个集合好了）花费 $log_2 (2k)$，所以一共是 $2k log_2 (2k) = O(klogk)$

## Array-based Kruskal’s algorithm
1. 边排序 $mlogm$ for $m$ edges.
$ m = n^2 , log(m) = 2 log(n)$
2. 排序以后每次从最小的那条边开始，判断两个顶点是不是在同一个集合里，不在一个集合里就 Union，否则跳过。一共 m 条边，所以要判断 2m 个顶点。(每次 find 的时间是 1)
3. 一共 n-1 次 union 操作 $O(nlogn)$
4. 所以 $O(mlog(n) + 2m + nlogn) =O(mlogn)$

##  Tree-based Find Kruskal’s algorithm
1. 判断 2m 个顶点的时候，每次 find 花费 O(logn)，所以是 $O(2mlogn)$
2. 不过 union 比较省力 $O(n)$
3. 所以 $O(mlogn + 2mlogn+ n) =O(mlogn)$

## Heap
1. findmin O(1)
2. inset/delete O(height)
3. make heap: $O(n)$
$\sum_h h \frac{n}{2^h} = O(n)$

## BFS DFB
$O(m+n)$

## DAG Topological Sort
$O(|V|+|E|)$

## Dijkstra’s Algorithm
$O(|E|log|V|)$

## Bellman-ford
$O(mn)$

# Subset sum 
给定一个集合，和一个边界值，从这个集合中找出一个子集，使得这个子集的和与这个边界值最接近。

Recurrence:
$$
OPT(j,w) = max
\left\\{
\begin{matrix}
OPT(j − 1, w), &  j \notin S' \\\\
w_j + OPT(j − 1, w − w_j), & j \in S'
\end{matrix} \right .
$$

RunTime: $O(nW)$

# Knapsack
背包问题与最大子集不同的就是这个集合里是$pair(w_j,v_j)$，我们要是value最大，而不是weight最大

Recurrence:
$$
OPT(j,w) = max
\left\\{
  \begin{matrix}
    OPT(j − 1, W), &  j \notin S' \\\\
    v_j + OPT(j − 1, W − w_j), & j \in S'
  \end{matrix} 
\right .
$$

# Segment least square
分段最小二乘
分段最小二乘重点在于，找到最后一条线段的起点。

Recurrence:
$$OPT(j)= min_{1 \leqslant i \leqslant j} \\{C+fit(p_i,...,p_j)+OPT(i−1)\\}$$

RunTime: $O(n^2)$

# Matrix Chain Multiplication
矩阵链相乘的问题
矩阵链相乘的最后一步总是，某两个矩阵相乘，所以关键在于，那个地方把矩阵链一分为二

Recurrence：
$$
m[i, j] = 
\left\\{
  \begin{matrix}
    0, &  i = j \\\\
    {% raw %}    min_{ i \leqslant k \leqslant j } ( m[i, k], m[k+1, j] + p_{i-1} p_k p_j ), & i < j  {% endraw %}
  \end{matrix} 
\right .
$$

RunTime: $O(n^2)$

# Optimal Binary Search Trees
二叉查找树的排列
根据二叉查找树的特点，其实所有节点可以看成是一列有序的，然后从中间挑一个作为根节点，使得这样形成的树最优

Recurrence：
{% raw %}
$$
C[i,j]  = 
\left\{
\begin{matrix}
0, &  \text{if } j < i \text{ (tree is empty)} \\
p_i, & \text{if } i = j \text{ (tree is single node)}\\
\sum_{a=1}^j P_a + min_r\{ C[i, r-1] + C[r+1, j] \}
\end{matrix} \right .
$$
{% endraw %}

RunTime: $O(n^2)$

# RNA Folding
RNA的分割配对
就是找最后一个基因是不是有配对，有配对的话，就按配对基因的位置，把整条RNA链分成两段。

Recurrence：
$$
OPT(i,j) = 
\left\\{
    \begin{matrix}
        0, & j - i ≤ 4 \\\\
        max
        \left\\{
            \begin{matrix}
                OPT(i,j-1) \\\\
                max_t\\{1 + OPT (i, t-1) + OPT (t+1, j-1)\\}$
            \end{matrix} 
        \right . & j - 1 > 4
    \end{matrix} 
\right .
$$

RunTime:$O(n3)$

画一个NxN的矩阵，一共有$n^2$个问题，每个问题都需要$O(n)$的时间来解决，所以是立方次的时间

# Sequence Alignment
两个字符串怎样排列差异最小

Recurrence:
$$
OPT(i,j) = min
\left\\{
    \begin{matrix}
        \alpha_{x_i y_j} + opt(i-1,j-1) \\\\
        \delta + opt(i-1,j) \\\\
        \delta + opt(i,j-1)
    \end{matrix} 
\right .
$$

RunTime: $O(mn)$ 
一共$mn$个子问题，每个子问题需要$O(1)$

***
上面是课堂内容。这里是分隔线。下面是作业里的内容
***

# Word Seperation
给定一个字符串$a_1, a_2, ... a_n$，把这个字符串分成单词，每种字串的配对都有相应的概率，这个概率是$word(a_i:a_j)$

Recurrence:
$$
OPT(i) = max_{1 \leqslant j \leqslant i} \\{ OPT(j) + word(a_j+1,a_i)  \\}
$$

Runtime: $O(n^2)$

# Bitonic Travelling Salesman
双调的旅行商问题

假设$1 \leqslant i \leqslant j$，$i$,$j$分别是向左，和向右走两条路径的最右端。
这里的确定recurrence的方式是，判断j旁边的那一个点的位置

{% raw %}
$$
OPT[i,j] = 
\left\{
    \begin{matrix}
      OPT[i, j-1] + d(j-1, j)     & \text{if }  i < j - 1 \\
      min_{1\leqslant k\leqslant j} \{ OPT[i,k]+d(k, j) \} & \text{if }  i = j - 1 \text{ or } i = j  \\ 
    \end{matrix} 
\right .
$$
{% endraw %}

# Line badness
给定每一行可容纳的字符数为$M$，如何排列字符，
关键在于什么时候开始最后一行。

Recurrence:
$$TotalBad(j) = min_{0\leqslant k \leq j}\\{ TotalBad(k) + LineBad(k+1,j) \\}$$

这是最后一行的badness:
$LineBad(i,j) = M - (j - i + \sum^j_{k =i}w_k)$ 







