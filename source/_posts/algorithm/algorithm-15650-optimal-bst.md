---
title: CMU 15650 - Optimal Binary Search Trees
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-19 18:12:45
---
CMU 15650 - Algorithm and Data Structure 

动态规划算法之 
Optimal Binary Search Trees
<!-- more -->

***
# Problem (Optimal Binary Search Trees). 
1. Sorted set of keys $k_1, k_2, ..., k_n$ 
2. Key probabilities $p_1, p_2, ..., p_n$
3. What tree structure has lowest expected cost?
4. Cost of searching for node $i: \text{cost}(k_i) = Depth(T,k_i) + 1$

点到根的距离
$Depth(T,k)$ is the distance from the root of key $k$, 

给定$n$个点，每个点被搜索到可能性不同，第$i$个点的可能性为$p_n$
搜索一个点的$Cost = p_i (Depth(T,k_i)+1)$

# Expected Search Cost
总Cost
{% raw %}
$
\begin{align*}
\text{Expected Cost of tree }
    & = \sum_{i=1}^n \text{cost}(k_i)p_i \\ 

    & = \sum_{i=1}^n (Depth(T,k_i) + 1) p_i \\ 

    & = \sum_{i=1}^n Depth(T,k_i) p_i + \sum_{i=1}^n  p_i  \\
    & = \left(\sum_{i=1}^n Depth(T,k_i) p_i\right) + 1 
\end{align*}
$

{% endraw %}





## Solution

Let $D(T,k) = Depth(T,k)$ for brevity. 
Let $T$ be an optimal tree that has root $k_r$ .

{% raw %}
$$
\begin{eqnarray}
C(T) &=& p_r + \sum_{a=1}^{r-1} p_a(D(T_l, K_a) + 1) +  \sum_{a=r+1}^{n} p_a(D(T_r, K_a) + 1) \\
&=& p_r + \sum_{a=1}^{r-1} p_a + \sum_{a=1}^{r-1} p_a(D(T_{left}, K_a)) + \sum_{a=r+1}^{n} p_a + \sum_{a=r+1}^{n} p_a(D(T_{right}, K_a)) \\
&=& p_r + \sum_{a=1}^{r-1} p_a + \sum_{a=r+1}^{n} p_a + \sum_{a=1}^{r-1} p_a(D(T_{left}, K_a))  + \sum_{a=r+1}^{n} p_a(D(T_{right}, K_a)) \\
&=& \sum_{a=1}^{n} p_a + C(T_{left}) + C(T_{right})
\end{eqnarray}
$$

{% endraw %}

所以可以用自底向上的方法来求最后树的Cost

## Recurrence
$C[i,j]$ := “cost of an optimal binary search tree on keys $k_i,...,k_j$.”

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

每次自底向上的循环中$r$都要从$i$到$j$遍历一遍
所以最后的$O(n) = n^2$ 




