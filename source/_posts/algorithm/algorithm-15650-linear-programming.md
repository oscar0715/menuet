---
title: CMU 15650 - Linear Programming
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-11-14 10:08:45
---
CMU 15650 - Algorithm and Data Structure 

Linear Programming
<!-- more -->

***

# Definition of LP
线性规划问题的定义：
1. $n$ 个变量 $x_1, x_2,... x_n$
2. $m$ 个关于这些变量的线性不等式
e.g., $3x_1 + 4x_2 ≤ 6$, 
$0 ≤ x_1 ≤ 3$
3. 关于这些变量的线性目标函数
2x_1 +3x_2 +x_3

目标：找到满足所有限制条件的，并且是目标函数最优的变量的值

写成公式：
1. 一个  $m \times n$ 的矩阵 $A$
2. 一个长度为 $m$ 的向量 $\vec{b}$
3. 一个长度为 $n$ 的向量 $\vec{c}$

目标是找到一个长度为 $n$ 的向量 $\vec{x}$ ，满足：
$$A \vec{x} ≤ \vec{b}$$
且使得下列目标函数最大：
$$\vec{c} · \vec{x} = \sum^n_{j=1}c_j x_j$$ 


更加一般化：
1. 上面的目标函数是最大化，如何求使得目标函数最小的情况
把目标函数重新写成 $\sum^n_{j=1} (- c_j) x_j$
2. 上面的限制条件都是小于号，如何处理大于的情况 $\vec{a_i}· \vec{x} ≥ b_i$
两边加上负号: $-\vec{a_i}· \vec{x} \leqslant -b_i$
3. 处理等号限制
那么同时加上 $\leqslant$ 和 $\geqslant$ 的条件

# Applications

## Maximum Flow
用线性规划解决最大流问题
{% raw %}
Maximize: <br>
$\sum_{(u,t)\in E} x_{ut}$ (流进t点的流量最大)<br><br> 

subject to: <br>
1. $0 ≤ x_{uv} ≤ c_{uv}$ for every $(u,v) ∈ E$   (流量小于最大容量)<br>
2. $\sum_{(u,v )∈E} x_{uv}= \sum_{(v,w )∈E} x_{vw}$  for all $v∈V$  (每个点的进流量和出流量相等)<br>

{% endraw %}

## Minimum-cost flow
问题，还是网络流问题，单位流量经过每条边都有 $q_e$ 的 cost，求要从 $s$ 到 $t$ 输送 $r$ 单位的流量的最小 cost 的路线

{% raw %}
minimize: <br>
$\sum_{(u,v)\in E} q_{uv}·x_{uv}$  （所有路线上的cost最小）<br>

subject to: <br>
1. $0 ≤ x_{uv} ≤ c_{uv}$ for every $(u,v) ∈ E$   (流量小于最大容量)<br>
2. $\sum_{(u,v )∈E} x_{uv}= \sum_{(v,w )∈E} x_{vw}$  for all $v∈V$  (每个点的进流量和出流量相等）<br>
3. $\sum_{(u,t) \in E} x_{ut} \geqslant r$ (运到t点的流量大于等于r)
{% endraw %}

## Shortest path

{% raw %}
minimize: <br>
$\sum_{(u,v)\in E} q_{uv}·x_{uv}$  <br>

subject to: <br>
1. $\sum_{(u,v )∈E} x_{uv}= \sum_{(v,w )∈E} x_{vw}$  for all $v∈V$  (每个点的进流量和出流量相等）<br>
2. $\sum_{(u,t) \in E} x_{ut} = 1$ (运到t点的流量为1)
{% endraw %}


## Maximum Bipartite Matching
{% raw %}
maximize: <br>
$\sum_{(u,v)\in E} x_{uv}$  <br>

subject to: <br>
1. $\sum_u x_{uv} \leqslant 1$  (每个点最多就找一个配对嘛，也就是出流量为1）<br>
2. $\sum_v x_{uv} \leqslant 1$ (同理，进流量最大也是1)
{% endraw %}

# Integer Linear Programming
整数线性规划，加上一个条件：$x_i ∈ {0,1}$ for all $i = 1,...,n $

## Minimum Vertex Cover
给定一个图，找出最小的顶点的子集，使这个子集可以连接到所有的边，一条边的一个顶点在这个集合里，就算连接到了。

{% raw %}
maximize: <br>
$\sum_{v\in V} x_v$  <br>

subject to: <br>
1. $ x_u+ x_v \geqslant 1$  for every$ {u,v}∈E$ (这条边至少一个顶点在这个集合里，也就是至少一个为1）<br>
2. $ x_u \in $ {0,1}  for all $u∈V$
{% endraw %}

# 小结
1. 很多问题都可以用线性规划来解决
2. 如果一个问题能够列出线性规划的公式，那么就可以用现成的一些solver来求解
3. 还有很多问题是整数规划问题
4. 如果是整数规划的话，那就不是polynomial-time algorithm


***
over！