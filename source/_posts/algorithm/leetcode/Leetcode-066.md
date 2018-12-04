---
title: Leetcode_066
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-04 17:13:00
---
Plus OnePlus One
<!-- more -->

***

### Problem
Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.

数字加一，链表玩过一次了，这次是数组


### Solution
代码如下
{% codeblock lang:java  %}
public int[] plusOne(int[] digits) {
  for (int i = digits.length - 1; i >= 0; i--) {
    if (digits[i] == 9) {
      digits[i] = 0;
    } else {
      digits[i] = digits[i] + 1;
      return digits;
    }
  }

  int[] d = new int[digits.length + 1];
  d[0] = 1;

  return d;
}
{% endcodeblock %}


*** 

Over！










































