---
title: Leetcode_90
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-27 10:41:00
---
Subsets II
<!-- more -->

***

### Problem
求集合的所有子集，集合可能有重复

### Solution 
backtrack
``` java
public class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        if(nums.length == 0) {
            return res;
        }
        
        Arrays.sort(nums);
        boolean[] flag = new boolean[nums.length];
        backtrack(new ArrayList<Integer>(), nums, flag, 0);
        return res;
    }
    
    public void backtrack(List<Integer> list, int[] nums, boolean[] flag, int start) {
        if( start <= nums.length) {
            res.add(new ArrayList<>(list));
        }
        

        for(int i = start; i < nums.length; i++) {

            if(i > start && nums[i] == nums[i-1] && !flag[i-1]){
                // 这理解解决相同的集合
                // 同一层递归里面，不添加相同的元素
                continue;
            }
            
            if (!flag[i]) {
                
                list.add(nums[i]);
                flag[i] = true;
                
                // 这里返回的是 i+1
                // 而不是 start + 1
                // 因为是排过序的
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










































