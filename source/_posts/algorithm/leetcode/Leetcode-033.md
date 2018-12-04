---
title: Leetcode_033
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-30 18:36:00
---
Search in Rotated Sorted Array
<!-- more -->

***

### Problem
给定一个有可能rotate过的有序array，和一个目标数字 target
(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).
找到 target 在这个 数组中的位置。

### Solution
二叉查找

``` java
public class Solution {
    public int search(int[] nums, int target) {
        return helper(nums, 0, nums.length - 1, target);
    }
    
    public int helper(int[] nums, int left, int right, int target) {
        if(left > right) {
            return -1;
        }
        
        int mid = left + (right-left) / 2;
        
        if(nums[mid] == target) {
            return mid;
        }
        
        int val1 = -1;
        int val2 = -1;
        
        // 有序的那一部分用二叉查找
        // 无序的那一部分，继续用 helper
        if(nums[left] < nums[mid]) {
            val1 = binarySearch(nums,left, mid - 1,target);
            val2 = helper(nums,mid + 1, right,target);
        } else {
            val1 = binarySearch(nums,mid + 1, right,target);
            val2 = helper(nums,left, mid - 1,target);
        }
        
        if(val1 == -1) {
            return val2;
        } else {
            return val1;
        }
    }
    
    // 二叉查找
    public int binarySearch(int nums[], int left, int right, int target) {
        if(left > right) {
            return -1;
        }
        
        int mid = left + (right-left) / 2;
        
        if(nums[mid] == target) {
            return mid;
        }
        
        if(nums[mid] > target) {
            return binarySearch(nums, left, mid - 1, target);
        } else {
            return binarySearch(nums, mid + 1, right, target);
        }
    }
}
```


*** 

Over！










































