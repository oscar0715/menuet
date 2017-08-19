---
title: CMU 15650 - Approximation Algorithms
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-12-05 10:52:45
---
CMU 15650 - Algorithm and Data Structure 

Approximation Algorithms
用来解决NP-C问题，当然不是彻底解决，而是得到一个近似的值
<!-- more -->

***

# Approximation Algorithms
The answer we get should never be too far from the optimal.

# Vertex Cover
先来看集合覆盖问题，怎么用 Approximation Algorithm 问题解决。

## Problem
回忆一下集合覆盖问题：
>Problem (Vertex Cover). 
Given undirected graph G, find the smallest set S of vertices so that every edge has an endpoint in S.

## Algorithm
```
S=∅
While there are edges left:
	Let (u,v) be any edge
	S = S ∪ {u,v} 
	// Add both u and v to S Remove every edge adjacent to either u or v
```

翻译一下：
1. 每次从图中挑一条边，
2. 把这条边的两个顶点加到集合 $S$ 中，
3. 再把这两个顶点都覆盖到的边删掉。

## 2-Approximation

>Theorem. 
This algorithm returns a vertex cover of at size ≤ twice the optimal.

![Two-Approximation](http://ogy8sh1ok.bkt.clouddn.com/approximation/vertex-cover-approximation.png "Two-Approximation")


最差的情况就是每次加入一条边的时候，这两个顶点都是没有别的覆盖的边，所以，有一个顶点是完全多余的。所以顶点数的最差的情况是最优情况时的两倍。

# Minimizing Makespan

## Problem
>Problem (Minimum Makespan).
Given $n$ jobs of length $t_1,...,t_n$, and $m$ machines $M_1,...,M_m,$ schedule the jobs on the machines to minimize the makespan.

>Def. The makespan is the total time to complete the jobs, running in parallel:

![Makespan](http://ogy8sh1ok.bkt.clouddn.com/approximation/makespan.png "Makespan")

把 $n$ 项任务分配到 $m$ 台机器上，计算怎么样能使花时间最少

## Algorithm
Greedy Algorithm:
1. Order the items arbitrarily.
把这些所有的任务任意顺序排序，得到一个列表
2. Go down the list of items, scheduling item $t_i$ on the machine that has been used the least so far.
根据这个列表的顺便分配任务，每次都把任务放到当前花费的时间最短的机器上。

一个例子
![Makespan Example](http://ogy8sh1ok.bkt.clouddn.com/approximation/makespan-example.png "Makespan Example")

![Makespan Example Result](http://ogy8sh1ok.bkt.clouddn.com/approximation/makespan-example-result.png "Makespan Example Result")

按照任务从左向右的顺序来分配任务
1. 把2分配到 $M_1$
2. 把3分配到 $M_2$
3. 把4分配到 $M_3$
4. 把6分配到 $M_1$
5. 把2分配到 $M_2$
6. 把2分配到 $M_3$

## Approximation Guarantee

Let $G(I)$ be the makespan obtained by this greedy algorithm for instance $I$.
Let $OPT(I)$ be the smallest possible makespan for instance $I$.

定义 $G(I)$ 是通过贪婪算法对问题 $I$ 得到的一个解。
定义 $OPT(I)$ 是问题 $I$ 的理论上的最优解

Goal: We want to prove something like:
For any instance $I$, $G(I) ≤ \alpha OPT(I)$. for some $\alpha $.

我们要证明这个算法得到的解是有一个边界值的，表达式是这样的 $G(I) ≤ \alpha OPT(I)$，我们要找到这个 $\alpha $。

显然 $\alpha $ 是大于1，不可能比最优解还小。我们继续找一个尽可能小的 $\alpha$ 的值

## Lower Bounds
Problem: we don’t know the optimal, so how do we compare against it?

\- Suppose we know that $B(I) ≤ OPT(I)$ for some function $B$.
\- If we can prove $G(I) ≤ \alpha B(I)$, then that immediately implies that $G(I) ≤ \alpha OPT(I)$.

![Lower Bounds Picture](http://ogy8sh1ok.bkt.clouddn.com/approximation/lower-bound-picture.png "Lower Bounds Picture")

有个问题就是我们不知道最优解是啥，那我们就用下限来计算。

## 2-Approximation
那么 Makespam 问题的下限怎么计算呢，现在我们有两条可以考虑

1. 最后的需要的时间肯定不会少于平均时间
$OPT(I) \geqslant \frac{1}{m} \sum_j t_j$
2. 最后需要的时间也不会少于最长的那么任务所需要的时间
$OPT(I) \geqslant max_j t_j$

我们需要先定义一些变量：
- Let $M$ be the machine with the maximum load in the greedy
solution.
$M$ 是花费时间最多的那台机器
- Let $t_j$ be the last job assigned to $M$.
$t_j$ 是最后分配到 $M$ 这台机器上的那个任务
- Let $L_i$ be the load the greedy algorithm places on machine $i$. (Note: $G(I) = L_M$)
$L_i$ 是每台机器最后要花费的时间，问题最后的解也就是$L_M$

在 $t_j$ 被分配到 $M$ 上之前，$M$ 是总 load 最小的机器，因此才会把$t_j$ 分配到 $M$ 上。
所以，Every other machine had load $≥ G (I) − t_j$ .

![Last Step of Greedy](http://ogy8sh1ok.bkt.clouddn.com/approximation/last-step-of-greedy.png "Last Step of Greedy")

我们继续证明，因为每一台别的机器上的load都大于 $≥ G (I) − t_j$. 那么我们通过求和，得到

$$ m(G(I) − t_j) \leqslant \sum_k L_k $$

两边同时除以 $m$
$$ G(I) − t_j \leqslant \frac{1}{m} \sum_k L_k$$

又因为 $\sum_k L_k \sum_k t_j$，所有机器的 load 总和等于所有任务所需时间之和嘛，所以：

$$ G(I) − t_j \leqslant \frac{1}{m} \sum_k L_k =  \frac{1}{m} \sum_k t_j  $$

我们前面有得到，最优解是要大于均值的，所以：

$$ G(I) − t_j \leqslant \frac{1}{m} \sum_k L_k =  \frac{1}{m} \sum_k t_j \leqslant OPT(I) $$

我们取首尾的部分:

$$ G(I) − t_j \leqslant OPT(I) $$

又因为，$t_j \leqslant OPT(I)$，因为 $OPT(I)$ 比最大那项还要大，当然比任意一项也大了。
所以：

$$ G(I) = (G(I) − t_j) + t_j  \leqslant OPT(I) + OPT(I) = 2 OPT(I) $$

## Better Approximation
还有更好的算法：
1. we sorted the jobs by decreasing length. 把任务根据花费的时间，从长到短排序
2. Place the big jobs first. 从大的任务开始分配，分配规则和之前一样

>Theorem. 
The Sorted-Greedy algorithm finds a solution of makespan $G(I) \leqslant \frac{3}{2} OPT(I)$ .

Let $t_1, . . . , t_n$ be the jobs, sorted by decreasing length.

![Another Lower Bound](http://ogy8sh1ok.bkt.clouddn.com/approximation/another-lower-bound.png "Another Lower Bound")

上图中 $t_{m+1}$ 是第 $m+1$ 个被分配的任务，所以在它分配的时候，机器上肯定有一个比它需要的时间更加长的任务（因为，分配顺序是从大到小）。所以:

$$ OPT(I) \geqslant 2t_{m+1} $$

同样
1. Consider machine $M$ that has the largest load $L_M$ at the end of the algorithm. 
设 $M$ 是算法得到的解中的 load 最大的那台机器
2. Let $t_j$ be the last job assigned to $M$.
$t_j$ 是最后分配到 $M$ 这台机器上的那个任务，我们上面已经分析了 {% raw %} $t_{m+1} ≤ \frac{1}{2}OPT(I)$ {% endraw %}

所以
$$L_M = (L_M − t_j) + t_j ≤ OPT(I) + \frac{1}{2}OPT(I) = \frac{3}{2} OPT(I) $$

***

Over!



