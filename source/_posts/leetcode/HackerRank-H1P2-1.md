---
title: HackerRank H1P2-1
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-05 13:21:0
---
动态规划，给定一个数组，求 a[j] - a[i]的最大值 (j > i)
<!-- more -->

***

### Problem
动态规划，给定一个数组，求 a[j] - a[i]的最大值 (j > i)

### Solution 
1. min保存到位置i之前的最小值
2. 比较a[i] 与 min 的大小
3. 如果a[i] 比较大，那么两者的差值是正的
4. 那么可以计算差值，更新result
5. 如果min更大，那么更新min就好了，反正result也不会变动
{% codeblock lang:java  %}
static int maxDifference(int[] a) {
	int result = -1;

	int min = a[0];
	for(int i = 1; i < a.length; i++) {
		if (a[i] > min) {
			result = Math.max(result, a[i] - min);
		}
		if (a[i] < min ) {
			min = a[i];
		}
	}
	return result;
}
{% endcodeblock %}

