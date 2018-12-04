---
title: Leetcode_198
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-16 13:21:00
---
House Robber

<!-- more -->

***

### Problem
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

意思就是给一个数组，求一个最大的sum，条件是，相邻的数不能同时加到sum里

### Solution
动态规划

新建一个sum数组，这个数组的存的是i点能rob的最大的值。

每前进一个点都计算一次当前的sum能到最大的值，存到sum数组里

sum[i]的取值有如下三种情况，取其中最大的
1. sum[i-1] 
如果用了i-1这个点的sum[i-1]，那么当前i这个点的值nums[i]就不能加上去了
2. sum[i-2]+nums[i] 
如果用了i-2这个点的sum[i-2]，加上i点的值nums[i]，这意味着放弃了i-1这个点的值nums[i-1] 
3. sum[i-3]+nums[i]
同理，如果用了i-3这个点的sum[i-3]，加上i点的值nums[i]，这意味着放弃了i-2和i-1这两点的值，nums[i-1]和nums[i-2] 

所以sum[i]，sum[i-1]，sum[i-2]，sum[i-3]动态联系在了一起

{% codeblock lang:java  %}
public int rob(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    if (nums.length == 1) {
        return nums[0];
    }
    if (nums.length == 2) {
        return Math.max(nums[0], nums[1]);
    }

    int[] sum = new int[nums.length];
    sum[0] = nums[0];
    sum[1] = nums[1];
    sum[2] = Math.max(sum[1], nums[0] + nums[2]);
    int i = 3;
    for (; i < nums.length; i++) {
        sum[i] = Math.max(Math.max(sum[i - 1], sum[i - 2] + nums[i]), sum[i - 3] + nums[i]);
    }

    return sum[i - 1];
}
{% endcodeblock %}


*** 

Over！










































