---
title: Leetcode_034
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-30 19:10:00
---
Search for a Range
<!-- more -->

***

# Problem
给定一个有重复的 Sort 过的数组。给定一个 Target 。
找出这个 Target 的 Range。

# Solution
两个二叉查找分别查找left 和 right

``` java
public class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = leftSearch(nums,0, nums.length-1,target);
        int right = rightSearch(nums,0, nums.length-1,target);
        
        return new int[]{left, right};
    }
    
    // 找左边的起始位置
    public int leftSearch(int[] nums, int left, int right, int target) {
        if(left > right) { 
            return -1;
        }
        
        int mid = left + (right - left) / 2;
        
        if(nums[mid] == target ) {
            if(mid - 1 < left) {
                return mid;
            } else {
                if(nums[mid - 1] < target ) {
                    return mid;
                } else {
                    // 如果左边还有和target相等的，那么就继续到左边找
                    return leftSearch(nums, left, mid - 1, target);
                }
            }
        }
        
        if(nums[mid] > target ) {
            return leftSearch(nums, left, mid - 1, target);
        } else {
            return leftSearch(nums, mid + 1, right, target);
        }
    }
    
    public int rightSearch(int[] nums, int left, int right, int target) {
        if(left > right) { 
            return -1;
        }
        
        int mid = left + (right - left) / 2;
        
        if(nums[mid] == target ) {
            if(mid + 1 > right) {
                return mid;
            } else {
                if(nums[mid + 1] > target ) {
                    return mid;
                } else {
                    // 如果右边还有和 target 相等的，那么就继续到右边找
                    return rightSearch(nums, mid+1, right, target);
                }
            }
        }
        
        if(nums[mid] > target ) {
            return rightSearch(nums, left, mid - 1, target);
        } else {
            return rightSearch(nums, mid + 1, right, target);
        }
    }
}
```


*** 

Over！










































