---
title: Leetcode_189
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-20 14:21:00
---
Rotate Array

<!-- more -->

***

### Problem
Rotate an array of n elements to the right by k steps.

For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].


### Solution
这个别人写的方法，太机智了………………

{% codeblock lang:java  %}
public void rotate(int[] nums, int k) {
    if (nums.length < 2) {
        return;
    }

    int length = nums.length;
    k = k % length;

    reverse(nums, 0, length - 1);

    reverse(nums, 0, k - 1);

    reverse(nums, k, length - 1);
}

private void reverse(int[] nums, int start, int end) {
    int tmp;

    while (start < end) {
        tmp = nums[start];
        nums[start] = nums[end];
        nums[end] = tmp;
        start++;
        end--;
    }
}
{% endcodeblock %}


*** 

Over！










































