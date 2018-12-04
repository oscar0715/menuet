---
title: Leetcode_266
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2016-07-09 23:40:00
---
Palindrome Permutation 

<!-- more -->

***

### Problem
Given a string, determine if a permutation of the string could form a palindrome.

For example,
"code" -> False, "aab" -> True, "carerac" -> True.

### Solution 
双指针

发现
1. 注释会影响速度！
2. while要比for快（怎么会这样）

{% codeblock lang:java  %}
public boolean canPermutePalindrome(String s) {
	if (s.length() < 1) {
		return true;
	}

	// 标记数组
	// 用基本类型boolean就好了，default为false
	// 如果用Boolean包装类，还要初始化
	boolean[] flag = new boolean[256];

	char[] arr = s.toCharArray();

	// 对每个字母都做标记
	int i = 0;
	while (i < s.length()) {

		flag[arr[i]] = !flag[arr[i]];
		i++;
	}

	// 如果字母数目为奇数的字母超过一个，则false
	int count = 0;
	i = 0;
	while (i < flag.length) {
		if (flag[i]) {
			count++;
			if (count > 1) {
				return false;
			}
		}
		i++;
	}

	return true;
}
{% endcodeblock %}
*** 

Over！










































