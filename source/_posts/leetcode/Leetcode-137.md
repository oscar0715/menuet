title: Leetcode_136
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-01 15:00:00
---
Single Number II
<!-- more -->

***

### Problem
Given an array of integers, every element appears three times except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?


### Solution 

普通方法
先排个序

但是好像此题的目的是用位运算的……

{% codeblock lang:java  %}
public int singleNumber(int[] nums) {
	if (nums.length == 1) {
		return nums[0];
	}
	
	Arrays.sort(nums);
	
	int i = 2;
	while(  i < nums.length  ) {
		if (nums[i - 2] == nums[i]) {
			i = i + 3;
		} else {
			return nums[i - 2];
		}
	}
	
	return nums[nums.length - 1];
}
{% endcodeblock %}

*** 

Over！










































