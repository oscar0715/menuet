---
title: Lecture30 Sorting II
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-21 12:54:30
---
Lecture30 Sorting II

<!-- more -->

<br>

***

<br>

# Quick sort

Recursive divide-and-conquer algorithm

Fastest comparesion-based sort for arrays

Worest O(n^2)
Vitually always O(n logn)

用在数组比较快
链表不一定比mergeSort快

1. Start with list I of n items 
2. Choose pivot item v from I 
3. Partition I into 2 unsorted lists I1 & I2
    - I1 all keys smaller than v's key
    - I2 all keys larger than v's key
    - Items with same key as v go into either list.
    - the pivot v does not go into wither list. 
4. Sort I1 recursively, yielding sorted list S1
5. Sort I2 recursively, yielding sorted list S2
6. concatenate S1, v and S1, yielding sorted list S

举个例子：
![Quick Sort O(n log n) ](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture30-SortingII/sort5.jpg "Quick Sort O(n log n) ")
上图就取每个array的第一个为pivot

但是，看下图，都选第一个有时候白花了很多时间。 O(n^2)
它明明排好序了啊
![Pivot Disastrous O(n^2)](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture30-SortingII/sort5-1.jpg "Pivot Disastrous O(n^2)")

那么有木有好一点的办法
**Choosing a pivot**
Randomly select an item form I as pivot
平均的长度是 一边四分之一 一边四分之三

On Average running  

## Quicksort on linked lists

假设 key 和 pivot相同的都放到I1，那如果整个链表值都是一样的那么又O(n^2)了

Better: 拆分为三个链表
I1, I2, Iv
Iv: contain pivot v & all items with same key as pivot

## Quicksort on arrays
In place

快速排序比别的O(n log n)算法快的原因是
外循环里的小循环都非常的small

![Quick Sort of Array](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture30-SortingII/sort5-2.png "Quick Sort of Array")

***

Over！

<br><br>
彩蛋！
![shoe](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture30-SortingII/shoe.png "shoe")

14年的男神苍老不少，但是那金花的衬衫还是帅爆了。





































