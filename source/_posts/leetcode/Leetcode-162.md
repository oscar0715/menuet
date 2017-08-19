---
title: Leetcode_162
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-04 18:55:00
---
Find Peak Element
<!-- more -->

***

### Problem
找到数组中比两隔壁都大的数字的index

### Solution 

``` java
public class Solution {
    public int findPeakElement(int[] nums) {
        
        if(nums.length == 1) {
            return 0;
        }
        // 判断头
        if(nums[0] > nums[1]){
            return 0;
        }
        
        // 找中间
        for(int i = 1; i < nums.length - 1; i++) {
            if(nums[i] > nums[i-1] && nums[i] > nums[i+1]) {
                return i;
            }
        }
        
        // 判断尾
        if(nums[nums.length-1] > nums[nums.length-2]) {
            return nums.length-1;
        }
        
        return -1;
    }
}
```

*** 

Over！










































