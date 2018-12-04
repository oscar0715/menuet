---
title: CMU 15650 - Splay Tree
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-12 09:54:45
---
CMU 15650 - Algorithm and Data Structure 

Splay Tree
<!-- more -->

***

# Binary Search Tree
二叉查找树的原则就是，左子树的所有节点都比右子树要小。
<a href="/ucb61b/lecture26-BalancedSearchTree/">Here is a link to Introduction of BST in UCB 61B</a>

二叉查找树的缺点是不稳定。所以我们需要一个更加稳定的结构，

# Splay Tree
## Introduction
1. no extra storage requirement 
2. simple to implement
3. Main idea: move frequently accessed items up in tree
    - 主要的思想就是，把经常被access的项往上移动。
4. amortized $O(log n)$ performance 
    - 摊销时间能够保证O(log n)
5. worst case single operation is $Ω(n)$
    - 我们说的是摊销时间保证，但是对于单个操作，不一定。

## Operation
<a href="/ucb61b/lecture34-SplayTrees/">Link to Introduction of Splay Trees in UCB 61B</a>

**splay(T, k)**: 
if $k ∈ T$, then move $k$ to the *root* using a particular set of transformations of the tree. 
Otherwise, move either the inorder successor or predecessor of $k$ to the root.

**Amortized logarithmic cost**
1. Self adjusting binary search trees。
    - 其实Splay操作就是不断地调整二叉查找树，让它变得平衡
2. Guarantee that any $m$ consecutive tree operations starting from an empty tree take $O(m log n)$ time, where $n$ is the maximum number of elements in tree at any time.
3. Another way of saying this is that the average cost of an operation (averaged over all the operations) is O(log n)
4. Call this amortized logarithmic cost. 摊销时间是对数阶的

**Splay Idea:**
1. The *find/insert/delete* operations can be written in terms of the “splay” operation. 总之，先做splay的操作
2. Splay is implemented by doing a standard BST “find” and then applying particular rotations *walking back up toward the root*. Splay操作呢，先要找到这个node，然后把它想办法已到root
3. Splay may actually make the tree worse, but over a series of operations the tree always gets better。有可能在某一次Splay操作以后，会看起来更加不平衡，但是在多次操作以后，肯定平衡起来。

## Amortized Analysis 
Concept：
1. Some operations will be costly, some will be cheap
2. $n$ general, when a sequence of $m$ operations has a total worst-case running time $O(m f(n))$, we say the amortized running time of each operation is $O(f(n))$