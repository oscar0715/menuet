---
title: Leetcode_263
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-24 19:16:00
---
Ugly Number

<!-- more -->

***

### Problem
Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.

Note that 1 is typically treated as an ugly number.

就是质因子只有2，3，5的数就是丑数

### Solution1 非递归 

{% codeblock lang:java  %}
public boolean isUgly(int num) {
	if (num <= 0) {
		return false;
	}
	while(num % 5 == 0) {
		num = num / 5;
	}
	while(num % 3 == 0) {
		num = num / 3;
	}
	while(num % 2 == 0) {
		num = num / 2;
	}
	if(num == 1) {
		return true;
	}
	  return false;
}
{% endcodeblock %}

### Solution2 递归

{% codeblock lang:java  %}
public boolean isUgly(int num) {
	if (num <= 0) {
		return false;
	}
	if (num == 1) {
		return true;
	}
	if (num % 2 == 0) {
		return isUgly(num / 2);
	}
	if (num % 3 == 0) {
		return isUgly(num / 3);
	}
	if (num % 5 == 0) {
		return isUgly(num / 5);
  	}
  	return false;
}
{% endcodeblock %}

*** 

Over！










































