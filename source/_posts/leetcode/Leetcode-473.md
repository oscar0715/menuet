---
title: Leetcode_473
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-27 16:43:00
---
Matchsticks to Square

<!-- more -->

***

# Problem
给定一个数组，看能不能分成四组，每组的和都相等。

# Solution 1

对数组排序以后，从大到小回溯，有优化作用
详见回溯主体中的注释

``` java
public class Solution {
    public boolean makesquare(int[] nums) {
        if(nums.length < 4) {
            return false;
        }
        
        int sum = 0;

        // 求和
        for(int num : nums) {
            sum += num;
        }

        if(sum % 4 != 0) {
            // 不能被4整除的话
            // 可以提前判死刑
            return false;
        }
        
        // 对数组排序以后
        Arrays.sort(nums);
        // 从数组的最后一位开始往前走
        // 也就是从最大的位置往前走
        return dfs(nums, new int[4], sum/4, nums.length-1);
        
    }
    
    private boolean dfs(int[] nums, int[] squre, int target, int index) {
        if(index < 0) {
            if(squre[0] == target && squre[1] == target && squre[2] == target) {
                return true;
            } else {
                return false;
            }
        }
        
        // 回溯主体
        int val = nums[index];
        for(int i = 0; i < 4; i++) {
            // 这个判断，帮助我们跳过很多可能
            // 如果从大到小排序，我们就能跳过更多的肯能性
            if( squre[i] + val <= target) {
                squre[i] += val;
                if(dfs(nums, squre, target, index - 1)) return true;
                squre[i] -= val;
            }
        }
        return false;
        
    } 
}

```







































