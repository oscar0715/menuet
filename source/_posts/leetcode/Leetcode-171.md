---
title: Leetcode_171
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-07 00:42:00
---
Excel Sheet Column Number

<!-- more -->

***

### Problem
Related to question Excel Sheet Column Title

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:
	A -> 1
	B -> 2
	C -> 3
	...
	Z -> 26
	AA -> 27
	AB -> 28 


### Solution
1. 26进制转化为10进制
2. 注意没有0


{% codeblock lang:java  %}
public int titleToNumber(String s) {
	char[] sArray = s.toCharArray();
	
	int i = s.length() - 1;
	int j = 0;
	
	int result = 0;
	while( i >= 0 ) {
	   result += Math.pow(26, i) * (sArray[j] - 'A' + 1);
	   j++;
	   i--;
	}
	return result;
}
{% endcodeblock %}


*** 

Over！










































