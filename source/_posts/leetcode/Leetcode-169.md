---
title: Leetcode_169
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-15 17:35:00
---
Majority Element

<!-- more -->

***

### Problem
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

求数组中超过半数的字母

### Solution
一个非常巧妙的算法
解释呢，不造轮子了，参考这里：
{% link Moore's voting algorithm http://blog.csdn.net/chfe007/article/details/42919017 Moore's voting algorithm %}

{% codeblock lang:java  %}
public int majorityElement(int[] nums) {
    int element = 0;
    int count = 0;
    int i = 0;
    while (i < nums.length) {
        if (count == 0) {
            element = nums[i];
            count++;
        } else {
            if (element == nums[i]) {
                count++;
            } else {
                count--;
            }
        }
        i++;
    }
    return element;
}
{% endcodeblock %}


*** 

Over！










































