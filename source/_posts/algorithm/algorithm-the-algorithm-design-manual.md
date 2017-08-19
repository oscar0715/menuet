---
title: The Algorithm Design Manual
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2017-01-02 19:27:45
---

不想刷题，半点都不想刷题。
如果刷题背后是算法，那干脆再研究研究算法好了。
leetcode 是术，而背后的算法才是道。

The Algorithm Design Manual 2nd
<!-- more -->

***

# Sorting and Searching
为什么排序那么重要：
1. 排序是许多其他算法的基础。很多算法的思想是建立在排序之上的，例如 divide-andconquer 和 randomized algorithm
2. 计算机的操作会频繁地用到排序。
3. 在计算机科学中，排序算法的研究是最深入的，有很多种排序算法。

***

## Application of Sorting
首先，我们要记住的是好的排序算法的时间复杂度 $O(nlogn)$ 

排序的应用：
1. Searching：
  	排序后可以用 Binary Search，时间复杂度 $O(nlogn)$ 
2. Closet pair：
  	排序后，所有的最接近的两个数字一定是相邻的，所以查找的复杂度是线性的，排序的时间复杂度 是 $O(nlogn)$ ，所以总的 时间复杂度 $O(nlogn)$ 
3. Element uniqueness:
	确定集合中的数字是否唯一时，也可以先用排序，然后用线性的时间复杂度就可以判断相邻的元素是否有重复。
4. Frequency distribution
	找出一个集合中出现最频繁的元素，排序后，可以从左到右数数。
5. Selection
	找出第 k 大的元素
6. Convex hulls
	给定一个点的集合，

思考题：
判断两个集合中是否有相同的元素，A 集合有 m 个元素，B 集合有 n 个元素，$m>n$

1. 算法1：对 A 排序
	排序复杂度 $O(mlogm)$。B 集合中每个元素到集合 A 中做二叉查找，所以查找的复杂度是 $O(nlogm)$。总复杂度 $O((m+n)logm)$
2. 算法1：对 B 排序
	算法同理，总复杂度 $O((m+n)logn)$。因为 $m>n$ 所以对小集合排序会比较快
3. 对两个集合都排序
	排序复杂度 $O((m+n)logn)$，分别对两个排序后集合从左向右搜索，丢弃较小的元素。总复杂度 $O(nlogn + mlogm + m + n)$

***

## Pragmatics of Sorting
排序顺序的依据：
1. 递增还是递减
2. 按照 key 排序，还是整个对象
3. 在出现 key 相同的时候，怎么处理？
	如果算法能够保留原来的排列是的顺序，那么我们说这个算法是 stable 的，不过在速度较快的算法中很少是 stable 的。
4. 非数字的数据怎么排序

***

## HeapSort 堆排序

选择排序：每次找出一个最小的元素
``` 
SelectionSort(A)
	For i = 1 to n do
		Sort[i] = Find-Minimum from A
		Delete-Minimum from A 
	Return(Sort)
```

1. 最普通的选择排序需要 $O(n^2)$ 的时间复杂度。
2. 堆排序
	但是如果我们用 heap 就可以把排序的复杂度降到 $nlog(n)$，因为每次找最小元素的时候我们不需要花费线性时间，而只要对数时间。

堆 Heap：
1. 最小堆，父节点一定比子节点要小
2. 堆结构不需要指针，要数组就可以。第 k 个元素的两个子节点是 2k 和 2k+1。
	这样的结构会节约很多空间：高度为 h 的二叉树，有不超过 $2^h$ 个节点，
3. 数组结构的堆意味中堆结构的中间不会有空缺。
4. 相对的，节约空间意味着损失灵活性，我们不能通过改变指针来把一颗子树移到另一个节点下。

思考：我们无法通过堆来快速找到某一个元素

构建堆：
1. 用到一个方法，叫做 bubble up 
	先把元素插入数组的末尾，如果这个元素比父节点小，就和父节点交换，这样一层一层往上换，直到符合堆的结构。一次 bubble-up 花费 $O(logn)$
2. 每插入一个元素都要做一次 bubble-up ，所以 n 个元素要花费 $O(nlogn)$
3. 但其实还有线性的方法
	因为越上面的元素其实 bubble up 的层数比下层的元素要少，所以分开考虑其实可以更加高效。
	$\sum_{h}h(n / 2^{h}) = n \sum h/{2^h} = O(n)$

提取最小元素：
1. 拿到堆顶以后，把最后一个元素拿到堆顶，做 bubble-down 

插入排序
扫描一个新的元素的时候，把它放到已排序的序列中合适的位置。
```
InsertionSort(A) A[0] = −∞
for i = 2 to n do j=i
while (A[j] < A[j − 1]) do swap(A[j], A[j − 1])
j=j−1
```

用一个二叉查找树 Balanced Search Tree 可以把时间复杂度降到 $O(nlogn)$





***
over！