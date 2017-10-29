---
title: CMU 15650 - Max-Flow Extensions
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-11-07 10:35:00
---
CMU 15650 - Algorithm and Data Structure 

网络图问题的拓展
Max-Flow Extensions
<!-- more -->

***

# Multiple source/sink 
第一种拓展是，有多个输出点和多个目的地。
1. 也就是多个source和多个sink
2. 每个sink都需要一定的流量流入，我们把这个流量定义为*需求*（demand）
3. 我们把输出点，也就是供应点的需求标记为一个负值

![Multiple source/sink](https://community.topcoder.com/i/education/maxFlow09.gif "Multiple source/sink")

这个时候我们可以定语这个网络流的两个**约束**:
1. 流量约束
对于边$e \in E$, $0 \leqslant f(e) \leqslant c_e$
2. 需求约束
对于点$v \in V$, $f^{in}(v) - f^{out}(v) = d_v$
当这个$d_v$是正数的时候呢，这个点就是一个目的地点，消耗了一定量的流量。
如果是负数的话，这个点是一个供应点，不消耗，反而生出了流量，$f^{out}(v)$ 大于$f^{in}(v)$

我们再定义：
1. $S$是需求为负数的点的集合，也就是供应点和出发点
2. $T$是需求为正数的点的集合，也就是需求点和目的地点

一个可行的流（供给等于需求）：
{% raw %}
$$\sum_{s \in S} (-d_s) =  \sum_{t \in T}d_t$$
{% endraw %}

总需求$D =  \sum_{t \in T}d_t$

接下来我们就要把问题转化为求最大流的问题了：
1. 添加一个source点$s'$，并添加边$(s',s)$,指向每一个点$s \in S$
边$(s',s)$的流量为$-d_s$，因为$d_s$是负数，所以这是个正数
2. 添加一个sink点$t'$，并添加边$(t,t')$,每一个点$t \in S$都指向$t'$
边$(t,t')$的流量为$d_t$

这样根据这个新的图中的capacity求得的最大流，就是所求的Multiple source/sink问题的结果了。

# Lower Bounds
另外一种拓展是，要求某些路径上至少要输送一定的流量，也就是给这些路径设置了一个大于零的流量下限

这时候要重新定义我们的 constraint:
1. 流量约束(注意不等式左边变成了大于零的下限)
对于边{% raw %}$e \in E$, $\mathscr{l}_e \leqslant f(e) \leqslant c_e${% endraw %}
2. 需求约束(这个没有变)
对于点 $v \in V$, $f^{in}(v) - f^{out}(v) = d_v$

定义初始值：
1. 对于每一条边
我们定义一个初始流量 $f_0(e)$ ，这个初始流量就等于下限值
$f_0(e) = \mathscr{l}e$
2. 对于每一个点
我们定义 $L_v = f_0^{in}(v)- f_0^{out}(v)$
所以，如果一个点的入边是有下限的，那么 $L_v$ 就是正的。
同样，如果一个点的出边是有下限的，那么就是负的。
当然，一个点可以涉及多条入边出边，最后的正负还是要看总值。
这个 $L_v$ 是可以抵消一部分的 $d_v$的 

根据这个$L_v$我们建立一个新的图
1. 需求约束
对于点 $v \in V$, $f^{in}(v) - f^{out}(v) = d_v - L_v$
2. 流量约束
对于边 $e \in E$, $0 \leqslant f(e) \leqslant c_e - \mathscr{l}_v $

所以这个新的图的意思就是，把这些下限值都当成已经发生了的，不在计算的范围内。
那么边的最大容量要变小，流入点的需求也变小（因为默认流入了一部分了）。
留出点能流出的流量也减小了。

然后我们就按照求最大流的算法计算出结果，最后在把下限值加上，就好了。


# Airline Scheduling
这个只能图解，看ppt吧