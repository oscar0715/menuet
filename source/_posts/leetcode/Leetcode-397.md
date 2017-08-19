---
title: Leetcode_397
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-13 13:21:00
---
Integer Replacement

<!-- more -->

***

### Problem
Given a positive integer n and you can do operations as follow:

If n is even, replace n with n/2.
If n is odd, you can replace n with either n + 1 or n - 1.
What is the minimum number of replacements needed for n to become 1?

Example 1:
	Input:
	8

	Output:
	3

Explanation:
8 -> 4 -> 2 -> 1

Example 2:

	Input:
	7

	Output:
	4

Explanation:
7 -> 8 -> 4 -> 2 -> 1
or
7 -> 6 -> 3 -> 2 -> 1

### Solution 
送分题

{% codeblock lang:java  %}
public int integerReplacement(int n) {
	return helper(n * 1.0, 0);
}

// 此处用的double，防止Integer.Max + 1 
private int helper(double n, int times) {
	if(n == 1) {
		return times;
	}
	if(n % 2 == 0) {
		return helper(n / 2, times + 1);
	} else {
		return Math.min(helper(n - 1, times + 1), helper(n + 1, times + 1));
	}
}
{% endcodeblock %}











































