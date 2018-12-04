---
title: Leetcode_486
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-11 00:17:00
---
Predict the Winner

<!-- more -->

***

### Problem
给定一个数组。两个选手分别从头尾两个数字中选一个，选完以后，这个数字就被剔除。
PlayerA 先选。
判断A有没有赢的机会。

### Solution 

利用递归动态规划

``` java
public class Solution {
    public boolean PredictTheWinner(int[] nums) {
        return PredictTheWinner(nums, 0, nums.length-1, 0, 0, true);
    }
    
    /* 
     * left 表示做指针的位置
     * right 表示右指针的位置
     * a 表示 player A 累积的分数
     * b 表示 player B 累积的分数
     * flag 是 true 时，轮到 player A 选择；反之，player B 选择。
     *
     */
    private boolean PredictTheWinner(int[] nums, int left, int right, int a, int b, boolean flag) {
    	// 当所有数字都选完了
    	// 判断大小
        if(left > right) {
            return a >= b;
        }
        
        if(flag) {
        	// 轮到 A 选择的时候
        	// 用 || 挑出一个对 A 有利的选择
            return PredictTheWinner(nums,left+1, right, a+nums[left], b, false) ||
                PredictTheWinner(nums,left, right-1, a+nums[right], b, false);
        } else {
        	// 轮到 B 选择的时候
        	// 要求 B 怎样选都是 A 赢，所以用 &&
            return PredictTheWinner(nums,left+1, right, a, b+nums[left], true) &&
                PredictTheWinner(nums,left, right-1, a, b+nums[right], true);
        }
    }
}
```











































