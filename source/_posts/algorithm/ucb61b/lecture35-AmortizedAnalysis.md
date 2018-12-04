---
title: Lecture35 Amortized Analysis
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-31 08:39:30
---
Lecture35 Amortized Analysis


<!-- more -->

<br>

***

<br>

# Amortized Analysis

Average time:
hash tables(Resize), disjoint sets, splay trees


# The Average Method
Suppose new hash table has one bucket. (slowest case)

Any operation that doesn't resize table takes O(1) time （只有resize是线性时间的）

N buckets & n items

suppose insert doubles size of table, (before adding new items) if n = N
load factor can't exceed one 
resizing takes <= 2n seconds

when resizeing done, N doubles, n + 1

以上是model

Construct an empty hash table, perform i inserts
Then, n = i and N is smallest power of 2 that is >= n

Total seconds for all resizing operations is 

2 + 4 + 8 + ... + N/4 + N/2 + N = 2N - 2

Cost of i inserts is <= i + 2N - 2 seconds
N < 2n = 2i 
O(5i-2) = `O(i)`

Average running time of insert O(i)/i = O(1)

Amortized running time of insert O(i);
worst-case time O(n) 





  























