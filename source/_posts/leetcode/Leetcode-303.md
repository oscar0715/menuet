---
title: Leetcode_303
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-24 19:40:00
---
Range Sum Query - Immutable

<!-- more -->

***

### Problem
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

Example:
  Given nums = [-2, 0, 3, -5, 2, -1]

  sumRange(0, 2) -> 1
  sumRange(2, 5) -> -1
  sumRange(0, 5) -> -3

Note:
1. You may assume that the array does not change.
2. There are many calls to sumRange function.

### Solution 

构造函数 O(n)
计算的时候 O(1)

``` java
public class NumArray {
    int[] sum;
    public NumArray(int[] nums) {
        
        if(nums == null || nums.length == 0) {
            return;
        }
        
        sum = new int[nums.length];
        
        sum[0] = nums[0];
        for(int i = 1; i < nums.length; i++) {
            sum[i] = sum[i-1] + nums[i];    
        }
    }
    
    public int sumRange(int i, int j) {
        if(i == 0) {
            return sum[j];
        } else {
            return sum[j] - sum[i-1];
        }
    }
}
```

*** 

Over！










































