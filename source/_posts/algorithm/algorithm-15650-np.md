---
title: CMU 15650 - NP Problems
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-11-28 09:52:45
---
CMU 15650 - Algorithm and Data Structure 

NP问题与计算难解性
<!-- more -->

***

# 多项式时间归约

能够用多项式个标准的计算步骤，加多项式次调用求解问题X的黑盒子来解决Y问题，我们就可以记作 $Y \leqslant _p X$ ，读作“Y多项式时间可归约到X”，也就是“X至少像Y一样难”

定理：假设 $Y \leqslant _p X$，如果X能在多项式时间内求解，那么Y也能在多项式时间内求解。反之亦然。

# 独立集与顶点覆盖
>独立集的定义：
在图 $G=(V,E)$ 中，如果顶点集合 $S \subseteq V$  中的任意两点之间都没有边，则称 $S$ 是独立的。 

寻找小的独立集是很容易的，比如一个顶点就构成了一个独立集，难度大的是寻找大的独立集。

>独立集问题的定义：
给定图$G$和数$k$，问： $G$ 包含大小至少为$k$的独立集吗？

为了说明NP问题的归约，我们考虑图论中的另一个基本问题：顶点覆盖

>顶点覆盖：
给定图 $G=(V,E)$ ，顶点集合 $S \subseteq V$ ，如果每一条边 $e \in E$ 至少有一个端点在 $S$ 中，则称 $S$ 是一个顶点覆盖。 

在顶点覆盖中，顶点在做覆盖，而边是被覆盖的对象。找到一个大的顶点覆盖是很容易的，例如整个顶点的集合就是一个顶点覆盖。难点在于找到一个小的顶点覆盖。

>顶点覆盖问题： 
给定图 $G$ 和数 $k$ ，问： $G$ 包含大小至多为 $k$ 的顶点覆盖吗？

![独立集合,顶点覆盖例子](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/1.png "独立集合,顶点覆盖例子")  

>引理：
给定图 $G=(V,E)$ ， $S \subseteq V$ ，那么 $S$ 是一个独立集，当且仅当它的补集 $V-S$ 是一个顶点覆盖。

证明：
首先设 $S$ 是一个独立集，考虑任意一条边 $e=(u,v)$ ，因为 $S$ 是独立集， $u$ 和 $v$ 不可能都在 $S$ 中，因而 $u$ 和 $v$ 至少有一个在 $V-S$ 中，得证每一条边至少有一个端点在 $V-S$ 中，所以 $V-S$ 是一个顶点覆盖集。
反过来，设 $V-S$ 是一个顶点覆盖集，考虑 $S$ 中的任意两个顶点 $u$ 和 $v$ ，如果它们之间有一条边 $e$ ，那么 $e$ 的两个端点都不在 $V-S$ 中，与假设 $V-S$ 是一个顶点覆盖集矛盾，因此 $S$ 中的任意两个顶点都没有边，所以 $S$ 是一个独立集。

所以我们可以得到两个问题双向的归纳：

>定理：
独立集 $\leqslant _p$ 顶点覆盖

证明：
如果有一个能够解顶点覆盖问题的黑盒子，那么通过问黑盒子： $G$ 是否有大小至多为 $n-k$ 的顶点覆盖，就能确定 $G$ 是否有大小至少为 $k$ 的独立集。

>定理：
顶点覆盖 $\leqslant _p$ 独立集

证明：
如果有一个能够解顶点独立集的黑盒子，那么通过问黑盒子： $G$ 是否有大小至多为 $n-k$ 的独立集，就能确定 $G$ 是否有大小至少为 $k$ 的顶点覆盖。

# 集合覆盖和集合包装
独立集和顶点覆盖是两种不同类型的问题

独立集可以看做包装问题：它的目标是“装入”尽可能多的顶点，要求服从一些限制条件（边）。这些限制条件试图阻止把顶点装入。

## 集合覆盖
而顶点覆盖可以看做一个覆盖问题：目标是用尽可能少的顶点“覆盖”图中的所有边。

顶点覆盖使用特殊的图的语言描述覆盖问题，其实还有更一般的覆盖问题——集合覆盖。在集合覆盖中，试图用一组较小的集合覆盖一个任意的对象集合。

>集合覆盖问题定义：
给定 $n$ 个元素的集合 $U$ , $U$ 的子集 $S_1$ ，...，$S_m$ 以及数 $k$ ，问在这些子集中有一组子集，它们的并集等于整个 $U$ ，且这一组子集至多含有 $k$ 个子集吗？

在顶点覆盖问题中，我们特殊地强调用与顶点关联的边集合覆盖图的边。而在集合覆盖中，我们试图用任意的子集覆盖一个任意的集合。直观上，顶点覆盖问题是集合覆盖问题的特殊情况。事实上，我们可以证明归约。

>定理：
顶点覆盖 $\leqslant _p$ 集合覆盖

证明：
假设我们能够进入一个解集合覆盖的黑盒子，考虑顶点覆盖的任意一个实例，给定图 $G=(V,E)$ 和 $k$ ，能怎样利用这个黑盒子帮助我们呢？

我们的目标是覆盖$E$中所有的边，所以构造集合覆盖的一个实例：

1. 它的基集 $U$ 等于 $E$ 
2. 在顶点覆盖问题中，每次取一个顶点，覆盖所有与它相关联的边
3. 于是，对每一个顶点 $i \in V$ ，设 $S_i \subseteq U$ 为 $G$ 中所有与 $i$ 关联的边，把 $S_i$ 加入集合覆盖的实例中。

>$U$ 能被集合 $S_1$ ，...， $S_m$ 中至多 $k$ 个覆盖，当且仅当， $G$ 有大小至多为 $k$ 的顶点覆盖。

{% raw %}
证明: <br>
1. 集合 $S_{i_1}$ ， $S_{i_2}$ ...， $S_{i_l}$ 是覆盖 $U$ 的集合，$l \leqslant k$，<br>
2. 那么 $G$ 中的每一条边都关联到顶点 $i_1$ ， $i_2$ ，...， $i_l$ 中的一个<br>
3. 所以 $\{ i_1，i_2，i_l \}$ 是 $G$ 中大小为 $l \leqslant k$ 的顶点覆盖<br>
<br>
1. 反之，如果 $\{ i_1，i_2，i_l \}$ 是 $G$ 中大小为 $l \leqslant k$ 的顶点覆盖<br>
2. 那么集合 $S_1$ ，...， $S_m$ 覆盖 $U$  <br>
{% endraw%}

## 集合包装

>集合包装问题：
给定 $n$ 个元素的集合 $U$ ， $U$ 的子集 $S_1$ ，...， $S_m$ ，以及一个数 $k$ 问这些子集中至少有 $k$ 个两两不想交吗？

>定理
独立集 $\leqslant _p$ 集合包装

<br>
***
<br>

# 可满足性问题（SAT）

## SAT 和 3-SAT问题
给定$n$个布尔变量，$x_1$ ，...， $x_n$ 的集合 $X$ ，每个布尔变量可以取值0或1（也就是假和真）。变量 $x_i$ 或它的否定 $\overline{x}_i$ 称作 $X$ 上的项，不同项的析取:

$$t_1 \bigvee t_2 \bigvee ... \bigvee t_l$$

称作字句（每个$t_1 \in \\{ x_1,x_2...x_n,\overline{x}_1,\overline{x}_2..\overline{x}_n\\}$）

现在规定说，变量的赋值满足一组子句，是什么意思呢？ 

1. $X$ 的真值赋值是每一个变量 $x_i$ 指定值 0 或 1 。
2. 换句话说，它是一个函数 $f:V \longrightarrow {0,1}$ . 
3. 毫无疑问，赋值 $f$ 给 $\overline{x}_i$ 指定与 $x_i$ 相反

如果在布尔逻辑的“或”规则下赋值子句 $C$ 的值为 1 ，这等价于要求 $C$， 中至少有一个项是 1 ，则称赋值满足 $C$ 。 

如果赋值满足每一个子句 $C_1$， $C_2$ ... $C_k$，使他们的合取 (与逻辑)：

$$C_1 \bigwedge C_2 \bigwedge ... \bigwedge C_k$$

的值为1，那么称该赋值满足这一组子句 $C_1$， $C_2$ ... $C_k$ 。

这种情况下称$f$是关于 $C_1$， $C_2$ ... $C_k$ 的一个满足的赋值，又称这一组子句 $C_1$， $C_2$ ... $C_k$ 是可满足的。

>举个例子，假设有三个子句
$(x_1 \bigvee \overline{t}_2)$ , $(\overline{x}_1 \bigvee \overline{t}_3)$ , $(x_2 \bigvee \overline{t}_3)$
令所有变量都是 1 的真值赋值 $f$ 不是满足的赋值，因为它不满足第二个子句。
令所有变量为 0 的真值赋值 $f'$ 是满足赋值

所以满足性问题的定义如下

>可满足性问题，记做SAT：
给定变量集 $X = \\{ x_1$，...，$x_n\\}$， 上的一组子句$C_1$， $C_2$ ... $C_k$，问存在满足的真值赋值吗？

SAT问题有一种特殊情况，就是限定所有子句的项的数量恰好是3个。这个问题成为三元满足性，记做 3-SAT.

>三元可满足性，记做 3-SAT.
给定变量集 $X = \\{ x_1$，...，$x_n\\}$ ， 上的一组子句 $C_1$， $C_2$ ... $C_k$ ，每一个子句的长为3，问：存在满足的真值赋值吗？

SAT 和 3-SAT 问题是是真正的基本的组合搜索问题，它们以非常“梗概”的方式包含了一个难计算问题的基本要素：
1. 必须做出 n 个独立决定，也就是对每一个$x_i$ 赋值
2. 使得满足一组约束

有多种方法单独地满足每一个约束，但是我们必须安排我们的决定使得所有的约束同时被满足。

## SAT 归约到 3-SAT

给定一组子句 $C_1$， $C_2$ ... $C_k$ ，每一个子句的长度都大于3

$$C_i = (t_1 \bigvee t_2 \bigvee ... \bigvee t_k)$$

我们挑出一个子句，添加一些变量 $y_i$， 一共 $k-3$ 个， 使得子句变成这样：
{% raw %}
$ (t_1 \bigvee t_2 \bigvee y_1)$<br>
$ (\overline{y}_1 \bigvee t_3 \bigvee y_2)$<br>
$ (\overline{y}_2 \bigvee t_3 \bigvee y_3)$<br>
...<br>
$ (\overline{y}_{i-3} \bigvee t_{i-1} \bigvee y_{i-2})$<br>
$ (\overline{y}_{i-2} \bigvee t_{i} \bigvee y_{i-1})$<br>
...<br>
$ (\overline{y}_{k-4} \bigvee t_{k-2} \bigvee y_{k-3})$<br>
$ (\overline{y}_{k-3} \bigvee t_{k-1} \bigvee t_k)$<br>
{% endraw %}

上面这些都是 括号之间都是“与”的关系

如果 $C_i = (t_1 \bigvee t_2 \bigvee ... \bigvee t_k)$ 满足，那么一定有一个 $t_i$ 的值是1，那么上述的式子也满足。我们只要设 $y_{i-2}$ 和之前的 $y$ 为 1，之后的都为0，那么式子就是满足的。

如果上述的式子满足，那么 $C_i = (t_1 \bigvee t_2 \bigvee ... \bigvee t_k)$ 也满足。我们假设 $C_i$ 不满足，那么所有的 $t_i$ 都是0。我们令所有的 $y$ 都是1，那么最后一个子句就不满足了，整个式子也不满足了，这就矛盾了。

## 把 3-SAT 归约到独立集

>定理:
3-SAT $\leqslant _p$ 独立集

证明：设有一个关于独立集的黑盒子，要解 3-SAT 的实例，实例由变量集 $X = \\{ x_1,..., x_n \\}$ 和 一组子句 $C_1$， $C_2$ ... $C_k$ 组成

考虑这个规约的关键是，认识到有两种概念上不同的方法来考虑 3—SAT 实例：
1. 对 $n$ 个变量中的每一个，做出独立的 $0/1$ 决定。
2. 从每一个子句中选出一个项，然后找一个真值赋值，使这些所有的项都为 1，从而满足子句。

第二种情况下，如果没有两个被选中的项“冲突”，那么就成功了。说两个项冲突的意思是：
1. 一个项等于 $x_i$ ，而另一个项 $\overline{x}_i$
2. 如果能不选中冲突的项，就可以找到一个真值赋值，使得从每一个子句中的项的值 1.

归约将基于上述对 3-SAT 实例的第二种看法，下面如何用图中的独立集描述 3-SAT 实例. 首先，构造图 $G = (V,E)$, 它由 $3k$ 个顶点组成 的 $k$ 个三角形组成。如下图所示：

![3—SAT 到独立集的归约](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/2.png "3—SAT 到独立集的归约")  

在第i个三角形中有三个顶点： 
1. {% raw %} $v_{i1}$, $v_{i2}$, $v_{i2}$，$i = 1,2..k$ {% endraw %}
2. 每个顶点之间用边相连。 
3. $v_{ij}$ 是 3-SAT 实例中子句 $C_j$ 的第 $j$ 个项

之前说，从每一个子句中选出一个项，也就是从每一个三角形中选出一个顶点。接下来设置阻止选择两个冲突的项：
1. 通过在图中增加一些边来描述冲突，在每一对标号对应的两个冲突的项的顶点之间添加一条边。
2. 现在是破坏了所有的大小为 $k$ 的独立集，还是仍然存在一个大小为 $k$ 的独立集呢？
3. 这取决于，我们从每个三角形中选出一个点以后，是否没有一对顶点是冲突的。

也就是说，要证明 3-SAT 是可满足的，当且仅当，我们可以构造出图 $G$ 有大小至少为 $k$ 的独立集。

证明：
首先
1. 如果 3-SAT 是可满足的，那么图中的每一个三角形至少包含一个标号的值为 1 的顶点， $S$ 就是由这些顶点组成的集合，我们说 $S$ 是一个独立集。
2. 因为这些顶点的值都是1，所以他们之间都没有边。

反过来：
1. 假设图 $G$ 有一个大小至少为 $k$ 的独立集 $S$ ， 那么首先 $S$ 的大小恰好为 $k$ 并且一定由每一个三角形中的一个顶点组成。
2. 那么我们只要把这些顶点的值设为1，3-SAT 问题就可满足了
 
## 归约的传递性

>定理：
如果 $Z \leqslant _p Y$，且 $Y \leqslant _p X$，则 $Z \leqslant _p X$

因此，我们可以得到
3-SAT $\leqslant _p$ 独立集 $\leqslant _p$ 顶点覆盖 $\leqslant _p$ 集合覆盖

<br>
***
<br>

# 有效证书和 NP 的定义

## 多项式运行时间

>一个计算问题的输入，被编码成有穷的二进制串 $s$ ，串 $s$ 的长度记做 $|s|$ .
把判定问题 $X$ 等同于由对它的答案为"Yes"的串组成的集合.
判定问题的算法 $A$ 接受输入串 $s$ 并返回 "yes" , "no". 
这个返回的值记做 "A(s)"，
如果对所有的串 $s$，$A(s) = yes$ 当且仅当 $s=X$，则称 A 解问题 X

>如果存在多项式函数 $p(·)$ 使得对每一个输入串 $s$，算法 $A$ 的计算在至多 $O(p(|s|))$ 步内终止，则称 $A$ 有多项式运行时间

## 有效证书 Certificate

问题的“验证算法”和解这个问题的算法是不一样的。

为了“验证”一个解，需要输入字符串 $s$, 以及另外一个“证书”串 $t$ 。这个证书$t$包含，$s$ 是 $X$ 的“yes” 实例，的证据。

如果下面的两条性质成立，则称 $B$ 是问题 $X$ 的有效验证程序
1. $B$ 是有两个输入变量 $t$ 和 $s$ 的多项式时间算法
2. 存在多项式 $p$ 使得对每一个输入串 $s$，$s \in X$ 当且仅当存在串 $t$，使得 $|t| \leqslant p(|s|)$ 且 $B(s,t) = yes$ 

## $NP$ 一个问题类

>定义 
$NP$ 是所有存在“有效验证程序”的问题的集合

>定理
$P \subseteq NP$

证明：
1. 假设问题 $X \in P$，也就是说有一个解 $X$ 的多项式算法 $A$
2. 为了证明 $X \in NP$，必须证明存在 $X$ 的有效验证程序 $B$

证明这个是很容易的，当输入 $(s,t)$ 的时候，验证程序 $B$ 只要返回 $A(s)$ 就好了。这里 $t$ 是没有用到的，因为 $A$ 可以在多项式时间内解决问题，所以 $B$ 这个验证程序也只要花费多项式时间。

在第一部分中，我们提到的问题都属于 $NP$ 
1. 三元可满足性 3-SAT ：证书 $t$ 是对所有变量的赋值，验证程序 $B$ 来计算这个子句在这些变量赋值下的值
2. 独立集：证书 $t$ 是至少有 $k$ 个顶点的集合，验证程序 $B$ 只要验证这些点之间都是没有边的
3. 集合覆盖：证书 $t$ 是一组集合中的 $k$ 个集合，验证程序只要验证这些集合的并集是不是等于基础集 $U$

我们不能确定:
> 在 $NP$ 问题中有不属于 $P$ 的问题吗？ $P = NP$ 吗？

一般来说我们觉得 $P \neq NP$. 毕竟找到一个解比验证一个解要难得多呀。

<br>
***
<br>

# NP 完全问题

## 定义
在 是否$P = NP$ 的问题没有进展的情况下，人们转向一个相对的，但更好处理的问题：那些是 $NP$ 中最难的问题？

“多项式时间可归约性”为我们提供了解决这个问题和洞察$NP$结构的途径

我们定义，
>$NP$ 中的每一个问题都能归约到 $X$，则称这样的 $X$ 是 $NP$ 完全问题

## 证明 NP 完全问题的通用策略

证明策略：
1. 证明 $X \in NP$
2. 选择一个已知的 $NP-complete$ 问题 $Y$
3. 证明 $Y \leqslant _p X $


那么我们怎么证明 $Y \leqslant _p X $ 呢？

考虑 $Y$ 的任意一个实例 $s_Y$ ，说明如何在多项式时间内构造 $X$ 的一个满足下述性质的实例 $s_X$ 
1. 如果 $s_Y$ 是 $Y$ 的 yes 实例，则 $s_X$ 是 $X$ 的 yes 实例
2. 如果 $s_X$ 是 $X$ 的 yes 实例，则 $s_Y$ 是 $Y$ 的 yes 实例

也就是说 $s_X$ 和 $s_Y$ 有相同的答案时。

<br>
***
<br>

# 哈密顿环问题
Hamiltonian Cycle Problem

>Problem (Hamiltonian Cycle). Given a directed graph $G$, is there a cycle that visits every vertex exactly once?

>哈密顿环问题：给定一个图 $G$，问：有没有一个环可以访问图中的所有的点，而且只经过一次。
如果存在这样的环，那么这个环就是哈密顿环。

>定理：哈密顿环是 NP-complete 问题

证明：
1. 给定一个有向图 $G=(V,E)$ ，证书是一个哈密顿环上所有顶点的顺序表。
2. 我们可以在多项式时间内，验证这个顺序表确实可以遍历所有的顶点，而且只访问一次。

> 3-SAT $\leqslant _p$ Hamiltonian Cycle

证明：
考虑一个 3-SAT 的实例，由 $X = \\{ x_1,..., x_n \\}$ 和 一组子句 $C_1$， $C_2$ ... $C_k$ 组成

归约的抽象的想法：
1. 用图结构（gadget）来表示这些 $X$ 中的变量（Gadget表示零件）
2. 同时用图结构来表示 $C_i$ 子句
3. 根据这些式子，想办法把这些图连接起来。Hook them up in some way that encodes the formula
4. 证明，当式子成立的时候，图中存在哈密顿环


***

怎么构造呢：
我们有 $n$ 个变量， $k$个Clause：
1. 我们构造出 $n$ 条路径 $P_1$，$P_2$ ... $P_n$，每一条路径如图3所示。
2. {% raw %} 每一条 $P_i$ 由 $b$ 个顶点 $v_{i1}$ ... $v_{ib}$ 组成。 {% endraw %}
3. $b$ 的取值略大于所有子句的总项数，取 $b=3k+3$ 
4. {% raw %}我们可以从左由 $v_{i1}$ 到 $v_{ib}$， 也可以从右 $v_{ib}$ ... $v_{i1}$ {% endraw %}

!["Gadget Representing the Variables"](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/3.png "图3 Gadget Representing the Variables")


如果这条线路是从左往右走的，那么 $x_i$ 取1，否则取0

每个子句添加一个顶点。 用来保存 3-SAT 实例是否可以满足，当且仅当有某个哈密顿圈的时候。如下如所示。

!["Hooking in the Clauses"](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/4.png "图4 Hooking in the Clauses")

***

路径构造完了以后，我们要把这些路径连起来，如图5所示：
1. {% raw %}我们先把前一条路径的第一个点连到下一条路径的第一个点和最后一个点，也就是连接 $v_{i1}$ 到 $v_{i+1,1}$ 和 $v_{i+1,b}$，$i=1,2..n-1$ {% endraw %}
2. {% raw %} 前一条路径的最后一个点同理，连接 $v_{ib}$ 到 $v_{i+1,1}$ 和 $v_{i+1,b}$，$i=1,2..n-1$ {% endraw %}
2. 额外添加两个顶点 $s$ 和 $t$，这两点之间相连 $t$ 到 $s$
3. {% raw %} $s$ 连到 $s_{11}$ 和 $s_{1b}$ {% endraw %}
4. {% raw %} $t$ 连到 $s_{n1}$ 和 $s_{nb}$ {% endraw %}

在这个图中，一共有 $2^n$ 条哈密顿环。
1. 因为在离开 $s$ 以后，进入 $P1$ 分别有从左和从右两条路线可以走。
2. 离开 $P1$ 以后，进入 $P2$。无论 $P1$ 是怎么走的，$P2$ 也有两条路线可以走。
3. 所以每条路线都是有两种选择的，一共 $n$ 条路线，所以一共有 $2^n$ 条哈密顿环

!["Connecting up the paths"](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/5.png "图5 Connecting up the paths")

然后我们连接对每一个子句定义的顶点 $c_j$，把每一条路径中的顶点位置，$3j$ 和 $3j+1$ 留给在子句 $C_j$ 中出现的变量。

!["Connecting up the paths. cont."](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/6.png "图6 Connecting up the paths. cont.")

我们之前定义了：如果这条线路是从左往右走的，那么 $x_i$ 取1，否则取0。
那么如⬆图6所示：
1. 根据$c_1$ 的在 $P_1$中的两个箭头的方向， 我们知道 $c_1$ 是沿着 $P_1$ 从右向左走的，所以 $c_1$ 中 的 $x_1$ 取值为 -1
2. 根据$c_2$ 的在 $P_1$中的两个箭头的方向， 我们知道 $c_2$ 是沿着 $P_1$ 从左向右走的，所以 $c_2$ 中 的 $x_1$ 取值为 -1

***

多看一个例子：

假设 $C_1 = x_1 \bigvee \overline{x_2} \bigvee x_3 $

那么 $C_1$ 中的三个变量的取值对应在图中的表示就是，如↓图7所示
1. $x_1$ 在 $P_1$ 中的两条箭头是从左向右
2. $x_2$ 在 $P_2$ 中的两条箭头是从右向左
3. $x_3$ 在 $P_3$ 中的两条箭头是从左向右

![""](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/7.png "图7")


***

到这里构造 3-SAT 问题的 Graph 表示就完成了，接下来我们来完成证明。

【Goal】：我们要证明，3-SAT 问题满足，当且仅当，G 有哈密顿环

第一步：证明，3-SAT 问题满足时，G 中存在哈密顿环。
我们再用⬆图7的例子，如果子句成立，则必须 $x_1 = 1$ 或者 $x_2 = 0$ 或者 $x_3 = 1$，也就是 $P_1$ 从左向右走，或者 $P_2$ 从右向左走，或者 $P_3$ 从左向右走， 才有可能访问得到 $c_1$ 这个变量。

第二步：证明，G中存在哈密顿环时，3-SAT 问题满足。
G中存在哈密顿环，则所有的 $c_j$ 都被访问到了，所以，每一个子句都是被满足的。

得证。

***

Hamiltonian Path

Hamiltonian Path: Does $G$ contain a path that visits every node exactly once?
问：$G$ 中是否存在一条路径，恰好可以只访问每个顶点一次？

Reduce Hamiltonian Cycle to Hamiltonian Path.
从哈密顿环到哈密顿路的归约

给定一个哈密顿环 $G$，任意选一个点，把这个点拆成两个顶点，得到图 $G'$，如↓图8所示
![""](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/8.png "图8")

那么哈密顿路只要从 $v'$ 出发，然后到 $v''$ 结束就好了。

所以，哈密顿路也是一个 NP-complete 问题

<br>
***
<br>

# 旅行商问题

Traveling Salesman Problem 也就是 TSP

>Problem (Traveling Salesman Problem). 
Given $n$ cities, and distances $d(i,j)$ between each pair of cities, does there exist a path of length $≤ k$ that visits each city?

>旅行商问题：
给定 $n$ 个城市，城市 $i$ 和 $j$之间的距离是 $d(i,j)$。
问：一个旅行商要访问所有城市，是否存在一条路线，使得他需要走过的距离小于k？

证明：

TSP ∈ NP: a certificate is simply an ordering of the n cities.

首先很容易证明 TSP 问题是一个 NP-Complete 问题。证书是，所有城市的顺序的排列，验证程序核实对应的路线总长度不超过给定的界限。

现在证明 
>哈密顿环 $\leqslant _p$ 旅行商问题

给定一个有向图 $G=<V,E>$。
给定一个 TSP 实例 $D$，$D$ 中有 $n$ 个城市，和 $n(n-1)$ 段距离。
当 $D$ 两个城市之间有边时，我们规定 $d(c_i,c_j) = 1$，否则 $d(c_i,c_j) = 2$

$
d(c_i,c_j) = 
\left\\{
\begin{matrix}
1, &  if edge (vi,vj) ∈ E \\\\
2, &  otherwise
\end{matrix} \right .
$

> $G$ 有一个哈密顿环 等价于 $D$ 有一条 长队小于 $n$ 的路径

证明

1. If $G has a Ham. Cycle, then this ordering of cities gives a tour of length $≤ n$ in $D$ (only distances of length 1 are used). 
这个显而易见
2. Suppose $D$ has a tour of length $≤ n$. The tour length is the sum of $n$ terms, meaning each term must equal 1, and hence cities that are visited consecutively must be connected by an edge in $G$. 
如果 $D$ 有一条长度 $≤ n$ 的路径，因为肯定走的都是长度为 1 的边，那肯定是每个城市都只访问一遍的。

所以 TSP 是 NP-Complete

<br>***<br>

# Graph Coloring Problem

## 图着色问题定义
>Problem (Graph Coloring Problem). 
Given a graph $G$, can you color the nodes with $≤ k$ colors such that the endpoints of every edge are colored differently?

>图着色问题
给定一个图 $G$，能不能用小于 $k$ 种颜色让每条边的两个顶点的颜色都不一样

>用数学语言表达就是：
A k-coloring is a function $f : V→\\{1,...,k\\}$ such that for every edge $\\{u,v\\}$ we have $f(u) \neq f(v)$.
If such a function exists for a given graph G, then G is k-colorable.

Special case of k = 2

How can we test if a graph has a 2-coloring?
Check if the graph is bipartite.

Unfortunately, for $k ≥ 3$, the problem is NP-complete.

>Theorem. 3-Coloring is NP-complete.

首先 三着色问题是一个 NP 问题
3-Coloring ∈ NP: A valid coloring gives a certificate.

然后我们再证明:
>3-SAT $≤_P$ 3-Coloring

## 证明 3-SAT $≤_P$ 3-Coloring
考虑一个 3-SAT 的实例，由 $X = \\{ x_1,..., x_n \\}$ 和 一组子句 $C_1$， $C_2$ ... $C_k$ 组成

第一步，对于每一个变量 $x_i$ ，我们都画两个节点，$x_i$ 和 $\overline{x}_i$，并把这两个节点连接起来。另外画三个特殊的节点 T, F, B，这三个点连成一个三角形，分别叫做 真节点（True），假节点（False）和基点（Base）

如下图所示


![""](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/9.png "图9")


第二步，把 B 和所有变量节点连接起来。如↓图所示：


![""](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/10.png "图10")


↑图10中的树结构有以下性质：
1. $x_i$ 和 $\overline{x}_i$ 必须有不同的颜色，
2. $x_i$ 和 $\overline{x}_i$ 和基点的颜色也不一样
3. T, F, B 三个节点的颜色也都不一样。
4. 根据上面三条，$x_i$ 和 $\overline{x}_i$ 中一个得到真节点的颜色和另一个得到假节点的颜色

举个例子：
假设 $C_1 = t_1 \bigvee t2 \bigvee t_3 $

对应的 $t_1$， $t2$， $t_3$ 中至少有一个是真节点的颜色

我们把一个这个子句画成这样：

![""](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/11.png "图11")

如果三个变量都为false，如↓图12所示。这张图中，基点是蓝色，真节点为绿色，假节点为红色，因为三个变量都是 false，那么也都是红色。

![""](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/12.png "图12")

根据上面提到过的性质，我们只能把各个点的颜色涂成这样，那么顶端最后一个点不管涂什么颜色，总会和相邻的一个节点冲突，所以三个节点都为假是不可行的。

![""](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/13.png "图13")

其实，只有当三个节点中的一个是真的时候，顶端的节点才可能是可以着色，

# Three-Dimensional Matching Problem

## 三维匹配问题定义

Three-Dimensional Matching 也就是 3DM

![""](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-np/14.png "图14")


>Given: Sets $X$, $Y$, $Z$, each of size n, and a set $T ⊂ X × Y × Z$ of order triplets.
给定三个集合 $X$, $Y$, $Z$。每个集合的大小都是 $n$。同时给出一个三元组集合 $T ⊂ X × Y × Z$

>Question: is there a set of $n$ triplets in $T$ such that each element is contained in exactly one triplet?
问：$T$ 中是否有 $n$ 个三元组使得 $X$, $Y$, $Z$ 中的每一个元素都只出现在一个三元组中。

显然 3DM 是属于 NP

## 证明 3-SAT $≤_P$ 3DM

同样，考虑一个 3-SAT 的实例，由 $X = \\{ x_1,..., x_n \\}$ 和 一组子句 $C_1$， $C_2$ ... $C_k$ 组成

对于每一个变量 $x_i$ 我们都创造一个图组件，包含以下内容
1. {% raw %} $A_i = \{a_{i1},...,a_{i,2k}\}$  {% endraw %} ——core
2. $B_i = \\{bi1,...,bi,2k\\}$ ——tips
3. {% raw %}$t_{ij} = (a_{ij},a_{i,j+1},b_{ij})$ {% endraw %}  ——TF triples

没写完，有空再来填坑






