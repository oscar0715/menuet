---
title: CMU 15650 - Network Flows
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-29 09:59:00
---
CMU 15650 - Algorithm and Data Structure 

Network Flows
网络流问题 
<!-- more -->

***

# Flow Network （流网络）
A flow network is a connected, directed graph G = (V , E ).
一个网络流是一个连接有向图

1. 每条边有一个capacity $c_e$，是一个非负整数
2. A single source $s∈V$. 起点
3. A single sink $t∈V$. 终点
4. No edge enters the source and no edge leaves the sink
起点无入边，终点无出边

**假设**
1. Capacities are integers. 每条边的容量为整数
2. Every node has one edge adjacent to it. 每个顶点都有相邻的边
3. No edge enters the source and no edge leaves the sink. 起点无入边，终点无出边

# Flow（流）
把从$s$ 到 $t$的流看作是一个function，$f$ ，这个function给每条边的指定了一个整数的流量。
$f(e)$就是经过$e$这条边的流量。


**Constraints on $f$ :**
1. 流量限制
$0 ≤ f (e) ≤ c_e$ for each edge $e$. 
2. 平衡限制
对于除了$s$ 和 $t$ 以外的顶点 $v$，有：
{% raw %}  
$\sum_{e \text{ into } v} f(e) = \sum_{e \text{ leaving } v} f(e) $
{% endraw %} 

**Notation:**
1. f的值：
$f(e) = \sum_{e \text{ out of } s} f(e)$
即从顶点$s$送出的流量
2. $f^{in}(v) = \sum_{e \text{ into } v} f(e)$
3. $f^{out}(v) = \sum_{e \text{ leaving } v} f(e)$
4. 于是平衡限制可以写成$f^{in}(v) = f^{out}(v)$

# Problem（最大流问题）
**Definition (Value)：**
The value $v(f)$ of a flow $f$ is $f^{out}(s)$.
我觉得意思就是一个图的流量都由$f^{out}(s)$来决定吧

**Problem (Maximum Flow)：** 
Given a flow network $G$, find a flow $f$ of maximum possible value.
给定一个图$G$，求$f$的最大值，也就是$f^{out}(s)$的最大值。起点最多能送出多少流量。

# Solution
## 贪婪算法
一开始用贪婪算法，每条路都送出能输送的最大流量。

## Residual Graph 残差图
给出残差图的定义residual graph $G_f$ 
1. $G_f$ 的顶点与 $G$ 相同
2. Forward edges：
在 $G$ 中，如果一条边 $e = (u,v)$ 的流量：$f(e)$ 小于容量 $c_e$， $f(e) < c_e$，
那么就在 $G_f$ 中这两点间添加一条边 $e' = (u,v)$ ，容量为 $c_e−f(e)$
2. Backward edges:
在 $G$ 中，如果一条边 $e = (u,v)$ 的流量：$f(e) > 0$，
那么就在 $G_f$ 中这两点间添加一条反向边 $e' = (v,u)$，容量为 $f(e)$

也就是说
1. 如果一条边的流量还没有到容量，那么在 $G_f$ 中就能再加一条边
2. 如果一条边是有流量的，在 $G_f$ 中，就用一条反向的边来把这个流量表示出来


## Augmenting Path
1. 我们定义 $P$ 是一条，在 $G_f$，从 $s$ 到 $t$ 的路径。
2. $bottleneck(P, f)$ 是在这条路径上的容量最小的那条边的容量
3. 如果 $bottleneck(P, f) > 0$，那么就意味着，在这条路径上，我们可以再输送一些流量。

```
augment(f, P):
b = bottleneck(P,f)
For each edge (u,v) ∈ P:
    If e = (u,v) is a forward edge:    // 判断是在G_f中判断的
        Increase f(e) in G by b   //add some flow // 操作是在G中操作的
    Else:
        e’ = (v,u)
        Decrease f(e’) in G by b  //erase some flow
    EndIf
EndFor
Return f
```

这个伪代码还是有点费解呀
我的理解是：
1. $f(e)$ 还是在原图 $G$ 中的，和 $G_f$ 没有关系的，得到的新的 $f'$ 也是在原图中
2. $G_f$ 只用来求Augmenting path

我们可以证明得到的新的 $f'(e)$ 是可行的流
1. 如果 $e=(u,v)$ 是一条正向的边
$f'(e) = f(e) + bottleneck(P,f) \leqslant f(e) + c_e - f(e) = c_e$
2. 如果 $e=(u,v)$ 是一条反向的边，也就是说在原图 $G$ 当中其实是 $(v,u)$， $ bottleneck(P,f)$ 最大不过 $f(e)$ , 因为这条边在 $G_f$ 中就是由 $f(e)$ 产生的
$f'(e) = f(e) - bottleneck(P,f) \geqslant f(e)- f(e) = 0  $

## Ford-Fulkerson Algorithm

```
MaxFlow(G):
    // initialize:
    Set f[e] = 0 for all e in G
    
    // while there is an s-t path in Gf:
    While P = FindPath(s,t, Residual(G,f)) != None:
        f = augment(f, P)
        UpdateResidual(G, f)
    EndWhile
Return f
```

就是每次根据原图 $G$ 的 $f$ ，构成新的 $G_f$ ，再由这个新的 $G_f$ 去更新原图 $G$ 的 $f$

## Running Time
1. 假设 $G$ 有 $m$ 条边，$G_f$ 有 $\leqslant 2m$ edges
2. 每次循环，在 $G_f$ 中用 BFS 或者 DFS 找一条通路，$O(m+n)$
因为是连通图，所以 $m > n/2$，$O(m + n) = O(m)$
3. The Ford-Fulkerson algorithm runs in O(mC) time.
$C$ 是 $G$ 中 $s$ 可以输出的的最大流量: $C = \sum_{e \text{ out of } s} c_e$
每次循环流量 $f$ 可最多增加1，总流量不可能大过 $C$ ，所以最多 $C$ 次循环就能找到最大的 $f$
4. The Ford-Fulkerson algorithm runs in $O(mC)$ time.
所以一共就是 $O(mC)$

# Cuts Analysis

## Cuts and Cut Capacity
**Definition**
The capacity(A,B) of an s-t cut (A,B) is the sum of the capacities of edges leaving A.

1. 将图 $G$ 中的点分为两部分，$A$ 和 $B$，$s \in A$, $t \in B$
2. 这就是一个 $s-t$ $cut$：$(A,B)$
3. $cut(A,B)$ 的最大容量为 $c(A,B) = \sum_{e \text{ out of } A } c_e$，即从 $A$ 流向 $B$ 的流量的总和

## Cut Theorem
**一条定理 Theorem**
Let $f$ be an $s-t$ flow and $(A,B)$ be an $s-t$ $cut$. 
Then $v(f) = f^{out}(A) − f^{in}(A)$.

证明：
1. 对于原点来说
$f^{in}(s) = 0$，所以$v(f) = f^{out}(s) - f^{in}(s)$
2. 对于 $A$ 中的其他的点 $v$ 来说
这些点都是 $G$ 中，所以根据 Network flow 的定义有，$f^{out}(v) - f^{in}(v) = 0$
3. 所以
我们得到 $v(f) = \sum_{v \in A} (f^{out}(v) - f^{in}(v))$
其实只有 $v=s$ 的时候差值才非零

对于一条的边 $e$ 来说：
1. 如果 $e$ 的两端都在 $A$ 中，那么 $e$ 在上述的求和公式中，一次起到加的作用，一次起到减的作用，抵消掉了。
2. 如果 $e$ 只有头在 $A$ 中，那么就代表流量流入 $A$ ，那么起到加的作用
3. 反之 $e$ 只有尾在 $A$ 中，则代表流量流出 $A$ ，起到减的作用
4. 如果两端都不在 $A$ ，那么对求和没影响

我们可以改写上述的式子
{% raw %}
$v(f) = \sum_{v \in A} (f^{out}(v) - f^{in}(v)) = \sum_{e \text{ out of } A} - \sum_{e \text{ into } A} =  f^{out}(A) - f^{in}(A)$
{% endraw %}

对于$B$来说则刚好相反
$v(f) = f^{in}(B) - f^{out}(B)$

**另一条定理 Theorem** 
Let $f$ be any $s-t$ flow and $(A,B)$ be any $s-t$ $cut$. 
Then $v(f) ≤ capacity(A, B)$.

证明：
{% raw %}
$
\begin{align*}
v(f )
    & = f^{out}(A) - f^{in}(A) \\ 
    & \leqslant f^{out}(A) \\ 
    & = \sum_{e \text{ leaving } A} f(e) \\
    & \leqslant \sum_{e \text{ leaving } A} c_e \\
    & = capacity(A,B)
\end{align*}
$
{% endraw %}

这条定理很有用，虽然它只是个不等式，但是不等式右边独立于任何一条具体的流 $f$。
也就是说明了，任何的流 $f$ 都的上限都不会超过任何一个 $cut$ 的capacity

## Max Flow = MinCut 
1. 如果在residual Graph $G_f$ 中无法找到一条从 $s$ 到 $t$ 的路径，那么此时得到的$f$，就是流量最大的 $f$ ，记为 $f^*$.
2. 此时，必定有一道{% raw %}$cut(A^*, B^*)${% endraw %}，使得{% raw %}$v(f) = capacity(A^*, B^*)${% endraw %}
3. 而且此时，每条从 $A$ 到 $B$ 的边都是饱和的，否则在 $G_f$ 就有一条正向边，那么还存在一条 Augmenting Path
4. 每条从 $B$ 到 $A$ 的边都是的流量都是 0，否则在 $G_f$，就有一条从 $B$ 到 $A$ 的反向边，也就是从 $A$ 到 $B$ 的边，那么又存在一条Augmenting Path了

{% raw %}
$$
\begin{align*}
v(f )
    & = f^{out}(A^*) - f^{in}(A^*) \\ 
    & = \sum_{e \text{ out of }  A^*} f(e) - \sum_{e \text{ into } A^*} f(e)\\
    & = \sum_{e \text{ leaving } A^*} c_e - 0 \\
    & = capacity(A^*,B^*)
\end{align*}
$$
{% endraw %}

所以我们可以看见，在残差图中的正向边和反向边是多么的重要的。
我们可以得到以下结论
1. Ford-Fulkerson Algorithm得到的流 $f^*$ 是最大流
2. 如果已知最大流的值，那么我们可以算出 $s-t$ $cut$ 的最小容量，时间复杂度为 $O(m)$
只需在最后得到的 residual Graph 中从 $s$ 开始用BFS或者DFS搜索，能达到的点属于{% raw %}$A^*$，剩下的点属于$B^*${% endraw %}
需要注意的是，可能有很多个这样的cut，上述方法只能找到其中的一条



## 总结一下
求一个 Min-capacity Cut的流程
1. 找到最大流 $f^*$
2. 根据这个最大流构建残差图 $G_{f^{*}}$
3. 从起点 $s$ 出发做BFS，能够搜索到的点都属于 $A^{*}$
4. 剩下的点属于 ${B^{*}}$
5. {% raw %}${A^{*}, B^{*}}${% endraw %}就是我们要找的Min-capacity Cut


