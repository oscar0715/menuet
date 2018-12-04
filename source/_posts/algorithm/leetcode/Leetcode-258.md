---
title: Leetcode_258
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-19 11:40:00
---
Add Digits
求数根

<!-- more -->

***

### Problem
Given a non-negative integer `num`, repeatedly add all its digits until the result has only one digit.

For example:

Given `num = 38`, the process is like: `3 + 8 = 11`, `1 + 1 = 2`. Since `2` has only one digit, return it.

Follow up:
Could you do it without any loop/recursion in `O(1)` runtime?


### Solution 
留两个引用
[Leetcode 258 : Add Digits](http://www.lrj.name/post-show/id/55d4a4d57388b9a59f000001 "Leetcode 258 : Add Digits")
[LeetCode：Add Digits - 非负整数各位相加](http://my.oschina.net/Tsybius2014/blog/497645 "LeetCode：Add Digits - 非负整数各位相加" )

``` java
public int addDigits(int num) {
    return num - 9 * ( (num - 1) / 9 );
}
```

*** 

Over！










































