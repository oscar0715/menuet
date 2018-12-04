---
title: Leetcode_179
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-25 09:30:00
---
Largest Number 
排序题，将给出数组中的所有数字组成一个最大的数

<!-- more -->

***

### Problem
Given a list of non negative integers, arrange them such that they form the largest number.

For example, given [3, 30, 34, 5, 9], the largest formed number is 9534330.

Note: The result may be very large, so you need to return a string instead of an integer.

### Solution
1. 利用Arrays.sort( String(), new Comparator<String>(){});
2. 比较数组


{% codeblock lang:java  %}
public String largestNumber(int[] nums) {
  String[] list = new String[nums.length];

  // int[] 转成 String[] 对象数组
  // 然后才能用 Arrays.sort
  for(int i = 0; i < nums.length; i++) {
      list[i] = String.valueOf(nums[i]);
  }
  
  // sort， 定义Comparator
  Arrays.sort(list, new Comparator<String>() {
     public int compare(String s1, String s2) {
         return (s2 + s1).compareTo(s1 + s2);
     } 
  });
  
  // 如果数组里都是0，直接返回0
  if (list[0].equals("0")) {
      return "0";
  }
  
  // 拼接
  StringBuilder result = new StringBuilder();
  for(String s : list) {
      result.append(s);
  }
  
  // StringBuilder 转 String
  return result.toString();
}
{% endcodeblock %}


*** 

Over！










































