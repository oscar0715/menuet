---
title: CMU 15650 - Subset sum
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-18 13:52:45
---
CMU 15650 - Algorithm and Data Structure 

动态规划算法之 
Subset sum
<!-- more -->

***

# Dynamic Programming

Similar to divide & conquer: 
Build up the answer from smaller subproblems

# Subset Sum Problem

**Problem (Subset Sum)**
Given:   
1. an integer bound $W$ , 
2. and a collection of $n$ items, 
3. each item with a positive, integer weight $w_i$,

find a subset S of items to:

maximize $\sum_{i\in S} w_i$, while keeping  

$\sum_{i\in S} w_i \leqslant W$

说人话就是：
1. 有一个数字集合 $S$ 和一个边界值 $W$ 
2. 在 $S$ 中挑出一些数字，使得这些数字的和尽可能地接近于 $W$

# Solution

##  Value of the OPT
Suppose for now we’re not interested in the actual set of items.
Only interested in the value of a solution
如果我们不在意这个数字集合具体有哪些数字，只是在意最终的这个接近于 $W$ 的和

## Optimal Notation

**Notation：**
1. Let $S'$ be an optimal choice of items 
( $S'$ 为最优解的集合)
2. Let $OPT(n,W)$ be the value of the optimal solution.
($OPT(n,W)$为最优解的集合中所有数字的和)


**Subproblems:**
1. To compute $OPT(n,W)$ : We need the optimal value for subproblems consisting of the first $j$ items for every knapsack size $0 \leqslant w \leqslant W$.
2. Denote the optimal value of these subproblems by $OPT(j,w)$

为了求解 $OPT(n,W)$ ，我们需要用到包含前 $j$ 个item的子最优解 $OPT(j,w)$

## Recurrence
状态转移公式:
$$
OPT(j,w) = max
\left\\{
\begin{matrix}
OPT(j − 1, w), &  j \notin S' \\\\
w_j + OPT(j − 1, w − w_j), & j \in S'
\end{matrix} \right .
$$

$ OPT(0,W) = 0 $ If no items 
$ OPT(j,0) = 0 $ If no space

1. 第一种情况，第 $j$ 个 item 不在这个 $S'$ 集合中，那么
  $OPT(j,W)=OPT(j−1,W)$
2. 第二种情况，第 $j$ 个 item 在这个 $S'$ 集合中，那么
  $OPT(j,W) = w_j +OPT(j−1,W−w_j)$
3. 特殊情况，第 $j$ 个 item 的$ w_j > W$，那么这个 item 必然不在这个 $S'$ 集合中，那么
  $OPT(j,W)=OPT(j−1,W)$

其实动态规划的关键，就是在当前这一步的时候，对这一步的情况做分类讨论。

## Pseudocode
{% codeblock lang:java %}
SubsetSum(n, W):
    Initialize M[0,r] = 0 for each r = 0,...,W
    Initialize M[j,0] = 0 for each j = 1,...,n
for j = 1,...,n: // for every row
    for r = 0,...,W: // for every column
        if w[j] > r: // case where item can’t fit
            M[j,r] = M[j-1,r]
        M[j,r] = max(  // which is best?
            M[j-1,r],
         w[j] + M[j-1, W-w[j]]
        )
return M[n,W]
{% endcodeblock %}

一行一行往上填

## Finding the Choice of Items
Follow the arrows backward starting at the top right:
<br>
{% asset_img backward.jpg  %}
![find items](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-subset-sum/backward.jpg "find items")
<br>
The items in the path 8, 5, 4, 2
每个斜箭头的尾部对应的item为在目标集合中的item


## Runtime
1. $O(nW)$ cells in the matrix.
2. Each cell takes $O(1)$ time to fill. （比较两个值就好了）
3. $O(n)$ time to follow the path backwards.
4. Total running time is $O(nW + n)=O(nW)$.

This is `pseudo-polynomial` because it depends on the size of the input numbers.


# Knapsack 背包问题

## Problem (Knapsack)
Given:   
1. an integer bound $W$ , 
2. and a collection of $n$ items, 
3. each item with a positive , integer weight $w_i$,
4. a value $vi$ for each weight

find a subset S of items to:

maximize $\sum_{i\in S} v_i$, while keeping  

$\sum_{i\in S} w_i \leqslant W$

>Difference from Subset Sum: want to `maximize value` instead of weight.

## Gready Solution
Idea: 
Sort the items by $p_i = v_i /w_i$   按单价排序
Larger $v_i$ is better, smaller wi is better.
If you were allowed to chose fractions of items, this would work.

This greedy algorithm doesn’t work for knapsack where we can’t take part of an item. 但是如果不能拆，单价排序可能不管用了。

举个例子，四件物品
1. $w = 1$, $v = 30$, $v / w = 30$
2. $w = 2$, $v = 40$, $v / w = 20$
3. $w = 3$, $v = 45$, $v / w = 15$
4. $w = 4$, $v = 100$, $v / w = 25$
5. 限制条件 $W = 6$

能拆的话选择1, 4 和一半的2  
$V = 30 + 100 + 40/2 = 150$

不能拆的话，只能选1，4
$V = 30 + 100 = 130$
但是更好的选择是2，4
$V = 40 + 100 = 140$


## DP for 0-1 Knapsack
那么用动态规划来做

Subset Sum 问题的状态转移方程：
$$
OPT(j,w) = max
\left\\{
\begin{matrix}
OPT(j − 1, W), &  j \notin S' \\\\
w_j + OPT(j − 1, W − w_j), & j \in S'
\end{matrix} \right .
$$

Knapsack背包问题的状态转移方程：
$$
OPT(j,w) = max
\left\\{
  \begin{matrix}
    OPT(j − 1, W), &  j \notin S' \\\\
    v_j + OPT(j − 1, W − w_j), & j \in S'
  \end{matrix} 
\right .
$$

比较一下只差了一个 $v_j$

***
over！