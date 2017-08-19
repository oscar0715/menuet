---
title: Leetcode_053
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-17 17:37:00
---
Maximum Subarray
<!-- more -->

***

### Problem
Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
the contiguous subarray [4,−1,2,1] has the largest sum = 6.

### Solution

动态规划

动态规划还是要上过课，才能更加轻松地理解呀。

``` java
public class Solution {
	public int maxSubArray(int[] nums) {
		
		// res 记录当前的最大值
		int res = nums[0];
		// maxToLast记录连到上一个数字为止的subArray的最大和
		int maxToLast = res;
		
		for(int i = 1; i < nums.length; i++) {
			// 看看另起炉灶比较大，还是和前面继续连着比较大
			maxToLast = Math.max(nums[i], maxToLast + nums[i]);

			// 看看要不要根据maxToLast记录连到上一个数字为止的subArray的最大和更新res
			res = Math.max(res, maxToLast);
		}
		return res;
	}
}
```

*** 

Over！










































