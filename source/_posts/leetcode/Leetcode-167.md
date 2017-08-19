---
title: Leetcode_167
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-03 00:59:00
---
167 Two Sum II - Input array is sorted  

<!-- more -->

***

### Problem
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2


### Solution 
简单版的 TWO SUM
双指针
{% codeblock lang:java  %}
public int[] twoSum(int[] numbers, int target) {
	int left = 0;
	int right = numbers.length - 1;
	
	int[] result = new int[2];
	
	while(left < right) {
		if (numbers[left] + numbers[right] == target) {
			result[0] = left + 1;
			result[1] = right + 1;
			return result;
		} else if (numbers[left] + numbers[right] > target) {
			right--;
		} else {
			left ++;
		}
	}
	
	return result;
}
{% endcodeblock %}

*** 

Over！










































