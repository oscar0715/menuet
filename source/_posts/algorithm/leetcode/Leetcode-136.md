title: Leetcode_136
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-14 21:00:00
---
Single Number
<!-- more -->

***

### Problem
Given an array of integers, every element appears twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?


### Solution 
时间复杂度O(n)

异或！   XOR！

还是你们会玩

{% codeblock lang:java  %}
public int singleNumber(int[] nums) {
	if(nums.length==1){
		return nums[0];
	}
	
	int i=1;
	int result = nums[0];
	while(i<nums.length){
		result = result^nums[i];
		i++;
	}
	return result;
}
{% endcodeblock %}

*** 

Over！










































