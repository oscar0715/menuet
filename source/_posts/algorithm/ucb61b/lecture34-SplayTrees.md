---
title: Lecture34 Splay Trees
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-30 20:24:30
---
Lecture34 Splay Trees


<!-- more -->

<br>

***

<br>

# Spaly Trees
Balanced binay search tree

All operations: O(log n ) time on average
Single operation: worst O(n)time 

***

# Tree Roatations
Splay trees are kept balanced with ratations.

Splay trees are not perfectly balanced

***

# Splay Tree Operations

## Object find (object k);
Let X be node where search ended, whether it contains k or not. 
Splay X up the tree through a sequence of ratation, so X become root.

1. X 变为root，下次查找会很快。
2. 让树重新balance

3 Case
1. X is left child of a right child 或者 right child of a left child
先转parent，再转grand parent
![case 1](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture34-SplayTrees/find1.jpg "case 1")
2. X is left child of a left child 或者 right child of a right child
先转grand parent，再转parent
![case 2](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture34-SplayTrees/find2.jpg "case 2")
3. X is child of the root
重复case 1 和 case 2 就来到了case 3
![case 3](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture34-SplayTrees/find3.jpg "case 3")
4. 一个栗子
![一个栗子](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture34-SplayTrees/find4.jpg "一个栗子")

## Object first(); Object last();
min: 一路向左
max: 一路向右

然后 Splay it to root

## void insert(Object k, Object v);
insert new entry(k, v) 
Splay new node to the root

## Object remove(Object k);
和普通的查找二叉树一样。
如果被删除的点，有左右节点，会复杂一点。

1. Let X be the node removed from the tree
2. Splay X'parent to root.  

![delete](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture34-SplayTrees/remove.jpg "delete")
这个地方的X指的是，找到大于被删除数的最小值，然后把这个最小值的parent Splay to the root













































