---
title: Leetcode_168
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-07 16:45:00
---
Excel Sheet Column Title

<!-- more -->

***

### Problem
Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 


### Solution 
进制转换

1. n-- 那一行是关键
{% codeblock lang:java  %}
public String convertToTitle(int n) {
	StringBuilder sb = new StringBuilder();

	while(n > 0) {
		int reminder = n % 26;
		n = n / 26;
		if (n > 0) {
			sb.append((char)('A' + reminder - 1 ));
		}

	}

	return sb.reverse().toString();
}
{% endcodeblock %}

*** 

Over！










































