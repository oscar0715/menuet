---
title: Leetcode_088
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-05 14:48:00
---
Merge Sorted Array
<!-- more -->

***

### Problem
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

### Solution 
数组题

只要涉及到数组，问题都在于，插入或者删除都很麻烦，那么如何避免O(n^2)就很重要。

还有人的思维方法，我们通常习惯于从头思考，有时候要试着从尾部开始思考

``` java  
public void merge(int[] nums1, int m, int[] nums2, int n) {
    // 新数组总长度为m+n-1
    int total = m + n - 1;
    int i = m - 1;
    int j = n - 1;
    while (i >= 0 && j >= 0) {
        if (nums1[i] > nums2[j]) {
            nums1[total--] = nums1[i--];
        } else {
            nums1[total--] = nums2[j--];
        }
    }

    // 到了负值，才算copy完
    if (i < 0) {
        while (j >= 0) {
            nums1[total--] = nums2[j--];
        }
    }

}
```

*** 

Over！










































