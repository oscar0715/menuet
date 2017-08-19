---
title: CMU 15650 - RNA Folding
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-24 09:46:45
---
CMU 15650 - Algorithm and Data Structure 

动态规划算法之 
RNA Folding
<!-- more -->

***
# Problem (Optimal Binary Search Trees). 

RNA is single stranded and folds up:
- G and C stick together
- A and U stick together

RNA folding rules:
1. If two bases are closer than 4 bases apart, they cannot pair.
距离太短不能配对
2. Each base is matched to at most one other base 
一对一配对
3. The allowable pairs are {U,A} and {C,G}
4. Pairs can not “cross.” 
If $(i,j)$ and $(k,l)$ are paired, we must have $i < k < l < j$.

Problem:
- Given: a string $r = b_1b_2b_3,...,b_n$ with $b_i ∈ {A,C,U,G}$
- Find: the largest set of pairs $S = {(i,j)}$, where $i,j ∈ {1,2,...,n}$ that satisfies the RNA folding rules.
- Goal: match as many bases as possible.
 
# Subproblems
根据最后一个基因的配对状态来确定子问题
1. 最后一个基因没有配对
$OPT(i,j)  = OPT(i,j-1)$
2. 最后一个基因和$i,j$间的$t$位置的基因配对了，因此把基因链分成了两部分$(i, t-1)$和$(t+1,j-1)$
$OPT(i,j)  = max_t\{1 + OPT (i, t-1) + OPT (t+1, j-1)\}$
这种情况下，我们要遍历$t$，从$i$到$j$，来找到最大值

所以得到如下公式



$$
OPT(i,j) = 
\left\\{
    \begin{matrix}
        0, & j - i ≤ 4 \\\\
        max
        \left\\{
            \begin{matrix}
                OPT(i,j-1) \\\\
                max_t\\{1 + OPT (i, t-1) + OPT (t+1, j-1)\\}$
            \end{matrix} 
        \right . & j - 1 > 4
    \end{matrix} 
\right .
$$

# Order to solve the subproblems
根据$i,j$画一个矩阵的话，我们先解决对角线上的值，因为此时$j-i$是最小的，所以这些子问题是最小单位的子问题，然后一层一层往外移动

# Pseudocode
``` javascript
Initialize OPT[i,j] to 0 for 1 ≤ i,j ≤ n 
for k = 5, 6,..., n-1 // interval length 定义间隔的距离
    for i = 1,2,..., n-k // interval start, i 间隔的开始值
        Set j = i + k // interval end, j 间隔的结束值

        // 从 j-i距离最小的开始（对角线）

        // find the best t
        // 遍历一遍
        best_t = 0
        for t = i,...,j-1:
            if rna[t] is complementary to rna[j]: 
                best_t = max(best_t, 1 + OPT[i,t-1]+OPT[t+1,j-1])
        EndFor
        // Either pair j with t or nothing
        OPT[i,j] = max(best_t, OPT[i,j-1])
        
    EndFor
EndFor
return OPT[1,n]
```

# Runtime
三次循环 $O(n3)$
1. 一共有$O(n^2)$ 个子问题 （$i \times j$）
2. 每个子问题都要遍历$t$, $O(n)$

# 回溯 backtrace
``` C
def rna_backtrace(Arrows):
    Pairs = [] # holds the pairs in the optimal solution
    Stack = [(0, len(Arrows) - 1)] # tracks cells we have to visit 
    while len(Stack) > 0:
        i, j = Stack.pop()
        if j - i <= 4: continue # if cell is base case, skip it 
        # Arrow = -1 means we didn’t match j
        if Arrows[i][j] == -1:
            Stack.append((i, j - 1)) 
        else:
            t = Arrows[i][j]
            Pairs.append((t, j)) # save that j matched with t 
            # add the two daughter problems
            if t > i: Stack.append((i, t - 1))
            Stack.append((t + 1, j - 1))
return Pairs
```

// 打个书签回溯下次再看














