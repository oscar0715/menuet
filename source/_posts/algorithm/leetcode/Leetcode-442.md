---
title: Leetcode_442
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-04 15:17:00
---
Find All Duplicates in an Array

<!-- more -->

***

### Problem
找出数组中所有的重复的数

### Solution 

``` java
public class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> list = new ArrayList<>();
        int[] hash = new int[nums.length+1];
        
        for(int i : nums) {
            if( hash[i]++ == 1) {
                list.add(i);
            }
        }
        
        return list;
    }
}
```











































