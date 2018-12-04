---
title: Leetcode_219
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-24 15:39:00
---
Contains Duplicate II
找一个数组里有没有重复的两个数字，这两个数字要求坐标距离小于等于参数k
<!-- more -->

***

### Problem
Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.

### Solution 

用map存坐标，然后比较。
但是比较慢
{% codeblock lang:java  %}
public boolean containsNearbyDuplicate(int[] nums, int k) {
   
    Map<Integer, Integer> map = new HashMap<Integer, Integer>();
   
    
    for(int i = 0; i < nums.length; i++) {
        if(map.containsKey(nums[i])) {
            if(i - map.get(nums[i]) <= k) {
                return true;
            }
        }
        map.put(nums[i], i);
    }
    
    return false;
}
{% endcodeblock %}


### Solution

这个思路很巧妙啊，也快一点儿

1. 把Set里的数的数量保持在k+1个，所以set里的数字的距离一定<=k
2. 鉴于Set的唯一性，如果添加失败了，就说明在k的距离之内，有重复的数，于是返回true

{% codeblock lang:java  %}
public boolean containsNearbyDuplicate(int[] nums, int k) {
    if(nums==null || nums.length<2 || k==0)
        return false;
 
    int i=0; 
 
    HashSet<Integer> set = new HashSet<Integer>();
 
    for(int j=0; j<nums.length; j++){
        if(!set.add(nums[j])){
            return true;
        }            
 
        if(set.size()>=k+1){
            // 如果size大小超出了k+1
            // 此处为 i
            // 从头开始删
            set.remove(nums[i++]);
        }
    }
 
    return false;
}
{% endcodeblock %}

*** 

Over！










































