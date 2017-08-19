---
title: CMU 15650 - Segmented Least Squares
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-19 15:06:45
---
CMU 15650 - Algorithm and Data Structure 

动态规划算法之 
Segmented Least Squares
<!-- more -->

***
# Segmented Least Squares Problem 
Another Dynamic Programming Problem
分段最小二乘

![](https://kartikkukreja.files.wordpress.com/2013/10/segmented.png?w=300&h=231)

**Problem**

Problem. Given a sequence of points $p_1$, . . . , $p_n$ sorted by their x-coordinate,find a partition $S_1$, ..., $S_k$ of the points to minimize:

$$C × k +   \sum_{i} fit(S_i)$$

1. $C$ is a given constant, the penalty of introducing a new line.
2. $fit(S_i)$ is the best least-squares fit of a line $l$ to the set of points $S_i$
3. k is not an input!

这个问题里，如果每两个点就加一条线，确实误差没有了，但是这不是一个好的解法，所以要尽可能少地划线。
设定 $C$ 为一个作为惩罚量的定值，$k$ 为最终直线的数量

# Least Squares Fit
最小二乘法的推算
The least squares fit is:
$$ fit(S)= (y_p −ax_p −b)^2 $$

where the line $y_p = ax_p + b$ is given by:

$$ 
a = \frac{|S|\sum_p x_p y_p − (\sum_p x_p)(\sum_p y_p) }{|S|\sum_p x_p^2-(\sum_p x_p^2)}
$$

$$
b = \frac{\sum_p y_p - \sum_p x_p}{|S|}
$$


# Recurrence
之前说过，动态规划就是要在当前情况下，做一个分类讨论。
那么这个问题的分类就是，以当前点 $p_j$ 为终点的直线的起点在哪里。
要确定这个起点，我们只能一个一个去试，所以：

$$OPT(j)= min_{1 \leqslant i \leqslant j} \\{C+fit(p_i,...,p_j)+OPT(i−1)\\}$$

所有 $OPT(1)$ 到 $OPT(j-1)$ 所有子问题的情况都要考虑一遍，所以说是multiple choices




