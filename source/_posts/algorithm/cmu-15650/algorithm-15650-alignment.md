---
title: CMU 15650 - Sequence Alignment
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-26 10:24:45
---
CMU 15650 - Algorithm and Data Structure 

动态规划算法之 
Sequence Alignment 序列对比
<!-- more -->

***

# Problem 
相关背景定义：
1. 给定两个字符串
{% raw %}
$
X = x_1 x_2 .. x+m \\  
Y = y_1 y_2 .. y+n
$
{% endraw %}
2. 两个表示两个字符串不同位置的集合
$
\\{1，2，3, ..., m\\} \\\\
\\{1，2，3, ..., n\\}
$
3. $M$ 是一个有序的配对（pair）的集合
$M$中不能有交叉的情况出现： 所以：如果$(i,j),(i', j') \in M$ 而且 $i < i'$ 那么$j< j'$

举个例子
>stop-
-tops

alignment $=\\{ (2,1),(3,2),(4,3) \\}$ 

接下来就是序列对比的定义了：
1. $\delta$ 代表 gap penalty, 也就是说如果要在一个string中加一个短横，也就是所谓的gap才能去和另一个string对齐，那么这个时候就要有一个gap penalty。
2. $\alpha$ 代表两个字符串$p, q$,在相同位置上的字母的不匹配的cost
对于每一个$(i,j) \in M$ 都会有一个 cost {% raw %}$\alpha_{x_i y_i}${% endraw %}
如果$i, j$ 位置上的两个字母相等，那么这个cost为零。
如果$\alpha_{pq} = 0$那么表示两个字符串再每一个相应的位置上的字母都是一样的。
3. $M$这个集合的总代价就是 $\delta$ 和 $\alpha$ 的和

然后我们的问题就是如何排列两个字符串来使得这个总代价最小
$\delta$ 和 $\alpha$ 是外部的参数。
有一个条件就是$\delta + \alpha_{pq} < 3 \delta$

# Design the algorithm
继续用动态规划的思想来解决问题

把问题一分为二，判断两个字符串的最后一个位置$(m,n)$是不是一个配对
1. $(m,n) \in M$
2. $(m,n) \notin M$

引申出一个定理：
如果$(m,n) \notin M$，那么$X$字符串中的的第$m$个位置和$Y$字符串中的第$n$个位置肯定有一个是没有对应的配对的字母的。
证明：
设$i< m$, $j < n$， $(m,j) \in M$，$(i,n) \in M$
这个时候就出现了交叉的配对$(m,j)$和$(i,n)$是交叉的。

那么我们就可以把问题一分为三：
1. $(m,n) \in M$
2. $X$字符串中的的第$m$个位置没有对应的字母（这个时候$X$比较长）
3. $Y$字符串中的的第$n$个位置没有对应的字母（这个时候$Y$比较长）

设$opt(i,j)$为问题的最小的cost。
1. 对应第一种情况
$opt(m,n) = \alpha_{x_m y_n} + opt(m-1,n-1)$
2. 对应第二种情况，我们在$X$的$m$位置上要付出一个gap penalty，$\delta$
$opt(m,n) = \delta + opt(m-1,n)$
3. 对应第三种情况，我们在$Y$的$n$位置上要付出一个gap penalty，$\delta$
$opt(m,n) = \delta + opt(m,n-1)$

根据以上的三种情况就可以得到这个问题的状态转移方程了
序列对比的最小costs，对于$i \geqslant 1$ and $j \geqslant 1$，有：
$$opt(i,j) = min \\{ \alpha_{x_i y_j} + opt(i-1,j-1), \delta + opt(i-1,j), \delta + opt(i,j-1)\\}$$

写的好看一点儿

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

Running Time:
一共有O(mn)个子问题，每个子问题需要O(1)，所以一共是O(mn)

然后从右往左回溯可以找到路径





