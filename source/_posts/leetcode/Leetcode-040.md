---
title: Leetcode_040
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 23:14:00
---
Combination Sum II
<!-- more -->

***

### Problem
给定一个数组，和一个整数 target，给出所有使和为target的所有组合

1. 数组中可能有重复的元素
2. 每个元素只能用一次
2. 所有元素都是正数

### Solution

backtracking

``` java
public class Solution {
    
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> current = new ArrayList<>();
    
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtrack(candidates, 0, target);
        return res;
    }
    
    public void backtrack(int[] candidates, int left, int target) {
        if(target == 0) {
            res.add(new ArrayList(current));
            return;
        }
        
        for(int i = left; i < candidates.length && target >= candidates[i]; i++) {
            if(i > left && candidates[i] == candidates[i-1]) {
                // 在同一轮循环里面不能出现两次
                // 意思就是说，不会有重复的list
                // 但是可以在不同的循环里，可以添加相同的元素
                continue;
            }
            current.add(candidates[i]);
            backtrack(candidates, i + 1 , target - candidates[i]);
            current.remove(current.size()-1);
        }
        
    }
}
```


*** 

Over！










































