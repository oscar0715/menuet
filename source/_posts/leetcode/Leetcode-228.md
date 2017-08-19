---
title: Leetcode_228
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-03 13:54:00
---
Summary Ranges

<!-- more -->

***

### Problem

Given a sorted integer array without duplicates, return the summary of its ranges.

For example, given [0,1,2,4,5,7], return ["0->2","4->5","7"].

### Solution
数组，也没什么难的

``` java
public class Solution {
    public List<String> summaryRanges(int[] nums) {
        
        List<String> list = new ArrayList<>();
        if(nums.length == 0) {
            return list;
        }
        
        int start = nums[0];
        int end = nums[0];
        
        for(int i = 1; i < nums.length; i++) {
            if(nums[i] > 1 + end) {
                if(start == end){
                    list.add(start+"");
                } else {
                    list.add(start+"->"+end);
                }
                start = nums[i];
                end = nums[i];
            } else {
                end = nums[i];
            }
        }
        
        if(start == end){
            list.add(start+"");
        } else {
            list.add(start+"->"+end);
        }
        
        return list;
        
    }
}
```

*** 

Over！










































