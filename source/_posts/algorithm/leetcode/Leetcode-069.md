---
title: Leetcode_069
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-08 18:54:00
---
Sqrt(x)
<!-- more -->

***

### Problem
Implement int sqrt(int x).

Compute and return the square root of x.

### Solution
{% codeblock lang:java  %}
public int mySqrt(int x) {
	
	// long 防止 mid * mid 溢出
	long left = 0;
	long right = x;


	while (left < right) {

		// right - left == 1 特别处理，否则死循环
		if (right - left == 1) {
			if (right * right == x) {
				return (int)right;
			} else {
				return (int)left;
			}
		}

		long mid = left + ((right - left) >> 1);

		if (mid * mid == x) {
			return (int)mid;
		} else if (mid * mid > x) {
			right = mid;
		} else {
			left = mid;
		}
	}

	return (int)left;
}
{% endcodeblock %}

*** 

Over！










































