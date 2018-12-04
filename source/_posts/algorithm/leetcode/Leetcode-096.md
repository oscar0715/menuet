---
title: Leetcode_96
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-30 14:13:00
---
Unique Binary Search Trees
<!-- more -->

***

### Problem
Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

给出n，求包含1到n的二叉搜索树的所有可能的结构的数量

这一题之所以是BST数的原因是：
题目想要我们给出的是给定 n 个节点，数出可能构成多少种形状。
反之，如果是普通的二叉树的话，假设两个val不同的节点，同一种形状就有两种可能构造方式。

### Solution 
动态规划

基本的思想是

给出n的时候
a<sub>1</sub> &nbsp;&nbsp; a<sub>2</sub> &nbsp; ... a<sub>k</sub> &nbsp; a<sub>k+1</sub> &nbsp; ... a<sub>n</sub>
以a<sub>k</sub>为根节点，左子树有k-1种可能，右子树有n-(k-1)-1种可能，两边相乘，则得到这种情况下总的可能性

(动态规划是要把子树节点为0看有1种可能)

``` java
public int numTrees(int n) {
    if (n == 0) {
        return 0;
    }
    int[] num = new int[n + 1];
    num[0] = 1;
    num[1] = 1;

    // 循环终止条件为 i <= n
    for (int i = 2; i <= n; i++) {
        // 节点数为n时的可能性为累加左子树节点数为0到n-1的所有情况的可能性
        // j < i 是因为根节点占据一个节点数，左子树的数量最多只有i-1
        // 相对应右子树节点数是 为 i-1-j
        for (int j = 0; j < i; j++) {
            // 以 j 为根节点的可能性为
            // 左子树的可能性和右子树的可能性相乘
            num[i] += num[j] * num[i - j - 1];
        }
    }
    return num[n];
}
```

*** 

Over！










































