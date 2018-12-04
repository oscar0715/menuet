---
title: Leetcode_349
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-28 14:40:00
---
Intersection of Two Arrays

<!-- more -->

***

### Problem
Given two arrays, write a function to compute their intersection.

Example:
Given nums1 = [1, 2, 2, 1], nums2 = [2, 2], return [2].

Note:
Each element in the result must be unique.
The result can be in any order.

### Solution 
送分题
{% codeblock lang:java  %}
public int[] intersection(int[] nums1, int[] nums2) {
  int length1 = nums1.length;
  int length2 = nums2.length;

  Set<Integer> set = new HashSet<Integer>();

  int i = 0;
  int j = 0;
  Arrays.sort(nums1);
  Arrays.sort(nums2);
  while (i < length1 && j < length2) {
    if (nums1[i] == nums2[j]) {
      set.add(nums1[i]);
      i++;
      j++;
    } else if (nums1[i] < nums2[j]) {
      i++;
    } else {
      j++;
    }
  }

  // set 转 数组
  int[] result = new int[set.size()];
  i = 0;
  for (Integer num : set) {
    result[i] = num;
    i++;
  }
  return result;

}
{% endcodeblock %}




*** 

Over！










































