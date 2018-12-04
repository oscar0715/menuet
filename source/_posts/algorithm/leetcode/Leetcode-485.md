---
title: Leetcode_485
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-11 00:17:00
---
Max Consecutive Ones

<!-- more -->

***

### Problem
给定一个数组，统计最长的连续的1的自字符串的长度。

### Solution 

``` java
public class Solution {
	public int findMaxConsecutiveOnes(int[] nums) {
		int max = 0;
		int count = 0;
		
		for(int i = 0; i < nums.length; i++) {
			if(nums[i] == 1) {
				count ++;
				max = Math.max(count,max);
			} else {
				count = 0;
			}
		}
		
		return max;
	}
}
```











































