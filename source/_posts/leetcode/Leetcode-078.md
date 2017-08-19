---
title: Leetcode_78
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-27 13:14:00
---
Subsets
<!-- more -->

***

### Problem
求集合的所有子集，集合无重复

比有重复的少一次循环，回溯时少一个条件：
**<a href="/leetcode/Leetcode-090/">Leetcode-090</a>**

### Solution 

backtrack
``` java
public class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        if(nums.length == 0) {
            return res;
        }
        
        boolean[] flag = new boolean[nums.length];
        backtrack(new ArrayList<Integer>(), nums, flag, 0);
        return res;
    }
    
    public void backtrack(List<Integer> list, int[] nums, boolean[] flag, int start) {
        if( start <= nums.length) {
            res.add(new ArrayList<>(list));
        }
        
        for(int i = start; i < nums.length; i++) {
            
            if (!flag[i]) {
                
                list.add(nums[i]);
                flag[i] = true;
    
                backtrack(list, nums, flag, i+1);
                
                list.remove(list.size()-1);
                flag[i] =  false;
            }
        }
    }
}
```


*** 

Over！










































