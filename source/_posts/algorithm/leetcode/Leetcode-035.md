---
title: Leetcode_035
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-08 15:55:00
---
Search Insert Position
<!-- more -->

***

### Problem
给定一个有序数组，和一个目标数字。
1. 如果目标数字在数组中，返回目标数字在数组中的位置
2. 否则，返回按顺序插入的下一个位置

### Solution
二叉查找

``` java
public class Solution {
	public int searchInsert(int[] nums, int target) {
		return binarySearch(nums, 0, nums.length - 1, target);   
	}
	
	public int binarySearch(int[] nums, int low, int high, int target) {
		if (low > high) {
			return low;
		}
		
		int mid = low + (high - low) / 2;
		
		if( nums[mid] < target) {
			return binarySearch(nums, mid + 1, high, target);
		} else if ( nums[mid] > target ) {
			return binarySearch(nums, low, mid - 1, target);
		} else {
			return mid;
		}
	}
}
```


*** 

Over！










































