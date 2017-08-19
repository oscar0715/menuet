---
title: Leetcode_268
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2017-04-04 15:47:00
---
Missing Number

<!-- more -->

***

### Problem
给定一个数组，长度为 n，
n 个数字从 0 到 n 这 (n+1) 个数中间取。
找出那个没有被取到的数字。

### Solution 
``` java
public class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int sum = (0 + n) * (n+1) / 2;
        
        for(int i : nums) {
            sum -= i;
        }
        return sum;
    }
}
```
*** 

Over！










































