---
title: Lecture31 Disjoint Sets
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-25 09:01:30
---
Lecture31 Disjoint Sets

<!-- more -->

<br>

***

<br>

# Disjoint Set  
No item is in more than one set.  互斥的集合

Collection of disjoint sets is called a `partition`

2 Operations:
Union: merge 2 sets into 1
find: takes an item and tells us what set it's in.

Application: Kruskal's alg (Graph)

# List Based Disjoint Sets
`the Quick-Find algorithm`

- Each set references list of items in the set
- Each item references set that contains it.

Find: O(1) time
Union: O(n) time slow

# Tree-based Disjoint Sets

`the Quick-Union algorithm`

Union: O(1)
Find: slower
Quick-union faster overall than quick Find

Each Set maintained as a tree
Data structure is a forest 
Each item is initially root of its own tree.

No child or sibling references -- only parent

True identity of each set recorded at the root.

Union: make the root of one set be child of root of other set

find: walk through 要遍历这棵树,和树的深度有关

Tricks: 
1. Keep items from getting too deep: At each root, we record size of tree (totall number)
Union: Make smaller one as subtree of larger one (`Union by size`)
两棵树深度都是n的情况下，Union以后深度才会是n+1