---
title: Lecture26 Balanced Search Tree
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-15 09:06:30
---
Lecture26 Balanced Search Tree

**参考:**
{% link 浅谈算法和数据结构: 八 平衡查找树之2-3树 http://www.cnblogs.com/yangecnu/p/Introduce-2-3-Search-Tree.html 浅谈算法和数据结构: 八 平衡查找树之2-3树 %}

<!-- more -->

***

### Two-Three-Four Trees
Pferfectly balanced tree

1. find, insert, remove, all in worst-case O(log n) time
2. Every node has 2, 3 or 4 children （除了叶子节点）
3. Every node stores 1, 2 or 3 entries 
4. numbers of children is entries + 1 or Zero

<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture26-BalancedSearchTree/23tree.png)<br/>

二叉查找树的拓展版本

### Object find( Object k );
图中是个2，3数，最多只能有3个children，然后最多2个entries
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture26-BalancedSearchTree/find.png)<br/>

### void insert( Object k );
1. 如果插入的地方，entries个数不满
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture26-BalancedSearchTree/insert1.png)<br/>

2. 如果满了 （这个例子还是2，3树）
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture26-BalancedSearchTree/insert2.png)<br/>

3. 如果这个节点满了，父节点没满
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture26-BalancedSearchTree/insert3.png)<br/>

4. 如果这个节点满了，父节点也满了
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture26-BalancedSearchTree/insert4.png)<br/>

反正一直往上拆

### remove（Object k）
0. Find Key k
1. 如果是叶节点，直接remove
2. 如果是在中间的节点，那么用下一个key最大的替换它

在插入的时候我们碰到的问题是一个node里的entry数满了，怎么办
remove遇到的问题相反，如果一个node里的entry数为零了，怎么办
（想办法把它弄到leaf节点里？？？
1. remove() 1-key node
从sibling节点里偷一个节点过来，操作是当，父节点，sibling节点中的entry转一个圈，然后父节点的一个entry就到当前节点了，父节点entry数不变，sibling节点entry少了一个，当前节点则多了一个，然后就可以愉快地remove了
2. 如果sibling节点也只有1个entry
从父节点偷一个过来entry过来，这样导致的结果就是，sibling里的那一个entry也会跟到当前节点来。sibling就毁灭掉了。两个子节点合并在了一起（fusion operation）
3. 如果父节点和sibling节点都只一个entry
那就把这三个都fusion在一起，然后这是唯一的一种数的深度降低的情况

栗子
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture26-BalancedSearchTree/remove.jpg)






***












