---
title: Leetcode_153
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-31 18:40:00
---
153 Find Minimum in Rotated Sorted Array
二分查找
<!-- more -->

***

### Problem
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.

### Solution 二分查找

``` java
public class Solution {
    public int findMin(int[] nums) {
        return binary(nums, 0, nums.length-1);
         
    }
    
    public int binary(int[] nums, int left, int right) {
        if(left > right) {
            return nums[left];
        }
        
        int mid = left + (right-left)/2;
        
        if(nums[left] > nums[mid]) {
            return binary(nums, left, mid);
        } else if(nums[mid] > nums[right] ){
            return binary(nums, mid+1, right);
        } else {
            return nums[left];
        }
    }
}
```


*** 

Over！










































