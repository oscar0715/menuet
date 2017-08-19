---
title: Lecture29 Sorting I
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-20 09:06:30
---
Lecture29 Sorting I
讲排序了！好激动！
<!-- more -->

<br>

***

<br>

# Insertion sort 插入排序

O(n^2) time

Invariant: S is sorted 

start with empty list S & unsorted list I of `n` items

    for ( each item x in I ) {
        insert x into S, in sorted order
        就是I按次序向S插入，然后插入的时候找对的位置
    }

1. If S is linked list, O(n) worst-case time to `find right position`
2. If S is array, O(n) worst-case time to `shift higher items over` (二分查找log n, 但是找到以后还是要插入，数组的插入呃呃呃)
3. If S  is a array, insertion sort is in-place.(在原数组上做就好了，不需要两个数组了，也就是不需要额外空间)
![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture29-SortingI/sort1.jpg)
如果一部分已经有序了，从后面的字符开始insert，会快很多。
proportion to inversions（顺序不对的数对）
数据结构的有时候就在于linked list和array之间的compromise
4. If S is a balanced search tree, O(n log n)

<br>

***

<br>

# Selection sort 选择查找

O(n^2)

start with empty list S & unsorted list I of `n` items
 
    for ( i = 0; i < n; i++) {
        item in I with smallest key
        Remove x from I.
        Append x to end of s
        每次都找到I中最小值的，直接插到S的末尾就好了。
    }

Best case 也需要 O(n^2)
因为每次都要找最小的嘛，逃不掉

其实每次查找最小的范围都在减小 从`n-1`到`1`，最后sum就是`(n * (n - 1)) / 2` 时间复杂度还是平方的。

如果是数组同样也能不用额外的空间 In place


<br>

***

<br>

# Heap sort 堆排序
Selection Sort where I is a heap

    Toss all items in `I` onto heap `h`   // 生成无序堆
    h.bottomUpHeap();    // 堆排序 O(n)
    for (i = 0; i < n; i++) {
        // 从小到大取出来
        x = h.removeMin(); // O(log n)
        Append x to the end of S;
    }

所以 `O( n log n)  `

In Place （二叉堆`反向`存在数组里） 注意是反向哦！

![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture29-SortingI/sort3.jpg)

Heap sort 适用于Array 而不是 linked list

<br>

***

<br>

# Merge Sort 归并排序

Merge 2 sorted lists into one sorted list in linear time

Q1 & Q2 两个有序queue
Q 为空的queue

    while (Q1 Q2 都非空) {
        item1 = Q1.front();
        item2 = Q2.front();
        把小的那个存入Q中 
    }
    concatenate remaining non-empty queue to end of Q

recursive divide-and-conquer algorithm
把一个大的数据结构一分为二

1. 把I 一分为二到 I1，I2 
2. sort I1 `recursively`, yielding S1,
3. sort I2 `recursively`, yielding S2,
3. merge S1 and S2 into S

这个图是从下往上看的！
![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture29-SortingI/sort4.jpg)

Merge Sort 适合 Linked List， 用merge的适合不需要多余的空间
不太适合array，因为需要多于的空间来做merge


<br>

***

<br>

清晰的参考
{% link 各种排序算法总结 http://www.jianshu.com/p/f5baf7f27a7e 各种排序算法总结 %}
{% link 堆排序 http://alexyyek.github.io/2014/12/07/heap/ 堆排序 %}












