---
title: Leetcode_080
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-11 12:15:00
---
Remove Duplicates from Sorted Array II
<!-- more -->

***

### Problem
有序整数数组去重，允许每个数字出现两次

### Solution
和基本办的数组去重相比。
多一个 count 来数每个数字是否出现了两次以上。
``` java
public class Solution {
	public int removeDuplicates(int[] nums) {
		if(nums.length < 3){
			return nums.length;
		}
		
		// current number
		int cur = nums[0];
		
		// count the number of the duplicates
		int count = 1;
		
		int length = 1;
		
		for( int i = 1; i < nums.length; i++ ) {
			if ( nums[i] == cur ) {
				count++;
				if(count <= 2) {
					nums[length] = nums[i];
					length++; 
				}
			} else {
				count = 1;
				nums[length] = nums[i];
				length++;
				cur = nums[i];
			}
		}
		
		return length;
	}
}
```

*** 

Over！










































