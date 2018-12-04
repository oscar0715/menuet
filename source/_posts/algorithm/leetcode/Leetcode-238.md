---
title: Leetcode_238
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-03 14:35:00
---
Product of Array Except Self
<!-- more -->

***

### Problem

Given an array of n integers where n > 1, nums, return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Solve it without division and in O(n).

For example, given [1,2,3,4], return [24,12,8,6].

主要就是要考虑 0 的情况

### Solution1
普通做法

{% codeblock lang:java  %}
public int[] productExceptSelf(int[] nums) {

	int zeros = 0;
	int product = 1;
	
	// count the number of 0
	// get the sum of all the numbers except 0
	for( int i = 0; i < nums.length; i++) {
		if(nums[i] == 0) {
			zeros++;
		} else {
			product *= nums[i];
		}
	}

	if (zeros > 1) {
		// If there is more than one 0
		// then the result is all 0s
		return new int[nums.length];
	}

	if (zeros == 1) {
		// case of one 0
		int[] result = new int[nums.length];
		for( int i = 0; i < nums.length; i++) {
			if (nums[i] == 0) {
				result[i] = product;
				return result;
			}
		}
	} else {
		// case of no 0s
		for( int i = 0; i < nums.length; i++) {
			nums[i] = product / nums[i];
		}
	}

	return nums;
}
{% endcodeblock %}

### Solution2
人家的做法

{% codeblock lang:java  %}
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    res[0] = 1;
    for (int i = 1; i < n; i++) {
        res[i] = res[i - 1] * nums[i - 1];
    }
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        res[i] *= right;
        right *= nums[i];
    }
    return res;
}
{% endcodeblock %}
*** 

Over！










































