---
title: Leetcode_217
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-20 10:39:00
---
Contains Duplicate
找一个数组里有没有重复的两个数字
<!-- more -->

***

### Problem
Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

### Solution 

1. 排序
2. 两两比较

O(n)

{% codeblock lang:java  %}

public boolean containsDuplicate(int[] nums) {
    if (nums.length < 2) {
        return false;
    }

    Arrays.sort(nums);

    for (int i = 1 ; i < nums.length ; i++) {
        if (nums[i] == nums[i-1]) {
            return true;
        }
    }
    return false;
}
{% endcodeblock %}

*** 

Over！










































