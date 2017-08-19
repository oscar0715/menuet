---
title: Leetcode_448
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-11 12:17:00
---
Find All Numbers Disappeared in an Array

<!-- more -->

***

### Problem
给定一个数组，数组长度为 n，数组中所有数字都落在 [1,n] 的范围内。

找出 [1,n] 范围内没有出现在数组中的数字

### Solution 

用一个长度为 n 的 boolean 数组作标记。

``` java
public class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> list = new ArrayList<Integer>();
        
        boolean[] flag = new boolean[nums.length+1];
        
        for(int i : nums) {
            flag[i] = true;
        } 
        
        for(int i = 1; i < flag.length; i++) {
            if(flag[i] == false) {
                list.add(i);
            }
        }
        
        return list;
    }
}
```











































