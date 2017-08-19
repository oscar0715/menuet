---
title: CMU 15650 - Skip List
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-10 15:21:45
---
CMU 15650 - Algorithm and Data Structure 

Skip List
<!-- more -->

***
![Skip List](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-skip-list/skip-list-1.png "Skip List")

# Perfect Skip Lists
我们熟悉的是单链表，但是单链表的搜索时间是$O(n)$
Skip List的目的是，把搜索某个节点的时间复杂度降低到$O(logn)$，以空间换时间。

Skip List 有以下特点
1. Keys in sorted order 有序
2. $O(logn)$ levels，层数是对数
3. Each higher level contains $\frac{1}{2}$ the elements of the level below it
每一层的节点数都比下一层的要少$\frac{1}{2}$
4. Header & Sentinel nodes are in every level 
Header节点 和 Sentinel节点在每一次层都有
5. Nodes are of variable size:
	- contain between 1 and O(log n) pointers
    - 每一个节点的层数是不一定的，可能是1，也可能是O(log n)，相应的每一层都有指向这一次层的下一个节点的指针
6. Pointers point to the start of each node 
7. Called skip lists because higher level lists let you skip over many items
叫Skip List的原因是，在高层的时候，你可以跳过一些不必要的节点，提高搜索速度。

# Find Operation
![Find](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-skip-list/skip-list-2.png "Find")

**Runtime:**
1. $O(logn)$ levels 
2. visit at most 2 nodes per level
    - 每一层最多就访问两个节点
    - 这个很有意思，如果你访问了超过了两个节点，那么其实你可以在更高层跳过多访问的这些节点
    - 但是这个的前提是，**Perfect** Skip List
3. search time is O(log n)

# Insert & Delete
Skip Lists are a randomized data structure: the same sequence of inserts / deletes may produce different structures depending on the outcome of random coin flips.
因为**Perfect** Skip List的太过于结构化，我们很难去update。所以我们把条件宽一些。

## Randomization
每次插入一个节点，我们用投硬币的方式来决定，这个节点，要不要往上涨一层，所以每次的概率都是50%
1. Allows for some imbalance
2. Expected behavior (over the random choices) remains the same as with perfect skip lists.
3. Idea: Each node is promoted to the next higher level with probability 1/2
	- Expect 1/2 the nodes at level 1 
	- Expect 1/4 the nodes at level 2
4. Therefore, expect number of nodes at each level is the same as with perfect skip lists. 我们的期望值是和perfect skip list是一样的
5. Also: expect the promoted nodes will be well distributed across the list

## Insert Demo
Search first:
![Find](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-skip-list/skip-list-insert-1.png )
Then insert
![Insert](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-skip-list/skip-list-insert-2.png "Insert")



## Delete Demo
<br/>
![Delete](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-skip-list/skip-list-delete-1.png )
![Delete](http://ogy8sh1ok.bkt.clouddn.com/algorithm-15650-skip-list/skip-list-delete-2.png "Delete")
<br/>


# Skip List Analysis
**Backwards Analysis:**
Consider the reverse of the path you took to find k:
You always move up if you can. 
(because you always enter a node from its topmost level when doing a find)
回溯一条路线的时候，原则就是尽可能的往上爬。

1. What’s the probability that you can move up at a given step of the reverse walk? 
    - $P = 0.5$
    - 每次能往上爬一层的概率都是0.5
2. Define $C(j)$ to be the expected number of steps to return to the start if we start at level $j$.
    - 假设现在处在j层，我们要回到最高层（这里最高层是第一层么？？）
3. Steps to go up $j$ levels = Make one step, then make either
	- $C(j-1)$ steps if this step went up because node was extended past level $j$ [Prob = 0.5]     
	(向上走，$j$ 要减一层)（这里也就是越高的地方，j越小，所以最高层j就是1吧）
	- $C(j)$ steps if this step went left because node wasn’t extended up past $j$ [Prob = 0.5] 
	(向左走，$j$ 保持不变)
4. Expected number of steps to walk up $j$ levels is: 
	$$C(j) = 1 + 0.5C(j-1) + 0.5C(j)$$

So: 
$2C(j) = 2 + C(j-1) + C(j)$
$C(j) = 2 + C(j-1)$
Expected number of steps at each level = 2
这样就算出了每层最多走两步

Expanding $C(j)$ above gives us: $C(j) = 2j$
Since $O(log n)$ levels, we have $O(log n)$ steps, expected





