---
title: CMU 15650 - Matrix Chain Multiplication
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-19 17:06:45
---
CMU 15650 - Algorithm and Data Structure 

动态规划算法之 
Matrix-Chain Multiplication
<!-- more -->

***


# Matrix-Chain Multiplication
A series of matrices $A_1$, . . . , $A_4$ need to be multiplied:

1. $A_1$ is $10 \times 100$ matrix
2. $A_2$ is $100 \times 5$ matrix
3. $A_3$ is $5 \times 50$ matrix
4. $A_4$ is $50 \times 1$ matrix

**Associativity of matrix multiplication**:
$A_1$$A_2$$A_3$$A_4$ is a $10 \times 1$ matrix.


// 这里latex的公式我表示很蛋疼，latex语法没错，但是html渲染不出来…
// Latex里下标的语法”_”两个一起出现时首先被Markdown解析成了斜体,再经过Mathjax渲染时就认定成了语法错误了。
// Reference: {% link Markdown和Mathjax的冲突 http://lisabug.github.io/2015/04/11/Conflict-between-Markdown-and-Mathjax/ Markdown和Mathjax的冲突 %} 

$(A_1(A_2(A_3A_4)))$
- $A_{34} = A_3A_4$ , 250 mults, result is $5 \times 1$
- $A_{24} = A_2·A24$, 500 mults, result is $100 \times 1$  
- $A_{14} = A_1A24$ , 1000 mults, result is $10 \times 1$
- Total is 1750

$((A_1A_2)(A_3A_4))$
- $A_{12} = A_1A_2$ , 5000 mults, result is $10 \times 5$
- $A_{34} = A_3A_4$ , 250 mults, result is $5 \times 1$
- $A_{14} = A12·A34$ , 50 mults, result is $10 \times 1$
- Total is 5300

**Implication:**
The multiplication "sequence" (parenthesization) is important!!
虽然相乘的结果都是一样的，但是乘的顺序对性能影响很大。


# Subproblems
{% raw %}
$ OPT(i,j)= min_{1 \leqslant t \leqslant j} OPT(i,t−1)+OPT(t,j)+r_i \times c_{t-1} \times c_j $ 
{% endraw %}

这个公式一脸懵逼

// 书上也没有对应的内容，ppt有错，还特别简陋，墙裂吐槽……
// 至少相关的符号，能不能标记清楚！
// 以下是看别人ppt理解的解法

# Solution
Reference :
{% link Lecture 12: Chain Matrix Multiplication https://home.cse.ust.hk/~dekai/271/notes/L12/L12.pdf Lecture 12: Chain Matrix Multiplication %}
## Step 1

Step 1: Determine the structure of an optimal solution (in this case, a parenthesization). 
确定一个最优解的结构，这里是怎么确定括号的位置，也就是各个元素是怎么组合的

**Decompose the problem into subproblems:**

1. For each pair $1 \leqslant i \leqslant j \leqslant n$, 
2. determine the multiplication sequence for 
{% raw %}
$A_i ..._j = A_i A_{i+1} .. A_j $ 
{% endraw %}
that minimizes the number of multiplications
也就是说，我们把这$A_i ..._j$标记为相乘以后的结果
3. 我们定义$A_i$是一个{% raw %} $p_{i-1} \times p_i${% endraw %} 的矩阵：
Clearly, {% raw %} $A_i ... _j$ is a $p_{i-1} \times p_j$ {% endraw %} maxtrix
那么显然，这是一个{% raw %}$p_{i-1} \times p_j${% endraw %}的矩阵

**High-Level Parenthesization for $A_i ..._j$**
1. For any optimal multiplication sequence, 
2. at the last step you are multiplying two matrices:
{% raw %}$A_i ..._k$ and $A_{k+1} ..._j$ {% endraw %} for some $k$
3. That is 
{% raw %}
$A_i ..._j = (A_i...A_k) (A_{k+1}.. A_{j}) = A_i ..._k A_{k+1} ..._j$ 
{% endraw %}

简单的说，一个矩阵序列的相乘，最后一步一定是两个矩阵在相乘。

Thus the problem of determining the optimal sequence of multiplications is broken down into 2 questions.
也就是说，这个问题的重点就在于，我们如何把它一分为二：
1. How do we decide where to split the chain (what is $k$)?
    - (Search all possible values of $k$ )
    - 我们怎么去找这个分割点，只能一个一个去找
2. How do we parenthesize the subchains 
{% raw %}
$A_i ..._k$ and $A_{k+1} ..._j$
{% endraw %}
    - Problem has optimal substructure property that
    {% raw %} $A_i ..._k$ and $A_{k+1} ..._j$ {% endraw %}
    must be optimal so we can apply the same procedure recursively
 
**Optimal Substructure Property:** 
1. If final “optimal” solution of $A_i ..._j$ involves splitting into {% raw %} $A_i ..._k$ and $A_{k+1} ..._j$ {% endraw %} at final step 
如果一个最优解的，是把矩阵序列分成了两个子问题。
2. then parenthesization of {% raw %} $A_i ..._k$ and $A_{k+1} ..._j$ {% endraw %} in final optimal solution must also be optimal for the subproblems “standing alone”
那么这两个子问题，各自也是最优解

## Step 2
Step 2: Recursively define the value of an optimal solution.

We will store the solutions to the subproblems in an array. 
存在一个二维数组中

For $1 \leqslant i \leqslant j \leqslant n$, let $m[i, j]$ denote the minimum number of multiplications needed to compute $A_i ..._j$
$m[i, j]$即为要得到$A_i ..._j$这个矩阵所需要的最少的相乘的次数

The optimum cost can be described by the following recursive definition:
所以状态转移方程可以如下所示：

$$
m[i, j] = 
\left\\{
  \begin{matrix}
    0, &  i = j \\\\
    {% raw %}    min_{ i \leqslant k \leqslant j } ( m[i, k], m[k+1, j] + p_{i-1} p_k p_j ), & i < j  {% endraw %}
  \end{matrix} 
\right .
$$

There are only possible values of so we can check them all and find the one which returns a smallest cost.
所以$k$也是要从$i$到 $j$都遍历一次，才能得到$m[i, j]$

## Step 3
Step 3: Compute the value of an optimal solution in a bottom-up fashion.

1. $m[1..n, 1..n]$ 是一个$n \times n$的表格
2. 要计算$m[i, j]$，我们必须已经知道$m[i, k]$和 $m[k+1, j]$了。
2. 两个子问题的长度都小于$j-1+1$,
3. 所以我们填表的时候，是按照长度的升序来完成的


**实例**
1. $m[1,2], m[2,3], m[3,4]... m[n-3,n-2], m[n-2,n-1], m[n-1,n] $
2. $m[1,3], m[2,4], m[3,5]... m[n-3,n-1], m[n-2,n]$
3. $m[1,4], m[2,5], m[3,6]... m[n-3,n]$
4. ...
5. $m[1,n-1], m[2,n]$
6. $m[1,n]$

## Step 4
Step 4: Construct an optimal solution from computed information – extract the actual sequence.
得到了最后的值，还要把具体的顺序找出来。
// 没在课程范围内要求，先到这里吧


## 总结
1. 问题的本质最后都是两个矩阵相乘，所有子问题都是怎么把这个矩阵序列一分为二
2. 填表的顺序很重要


***
Over！












