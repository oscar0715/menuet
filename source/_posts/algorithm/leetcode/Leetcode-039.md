---
title: Leetcode_039
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 22:25:00
---
Combination Sum
<!-- more -->

***

### Problem
给定一个无重复数组，和一个整数 target，给出所有使和为target的所有组合

1. 数组中的元素可以重复使用
2. 所有元素都是正数，这个很重要，不然没法做了

### Solution

backtracking

``` java
public class Solution {
    
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> current = new ArrayList<>();
    
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtrack(candidates, 0, target);
        return res;
    }
    
    public void backtrack(int[] candidates, int left, int target) {
        if(target == 0) {
            res.add(new ArrayList(current));
            return;
        }
        
        // 这里 for 循环的判断条件
        // 里面有 target >= candidates[i] 
        // 不符合的话，减掉以后target就负数了，后面就不用跑下去了
        for(int i = left; i < candidates.length && target >= candidates[i]; i++) {
            current.add(candidates[i]);
            // 这里的下一层递归的起始位置还是 i ，这样可以重复使用元素。
            backtrack(candidates, i , target - candidates[i]);
            current.remove(current.size()-1);
        }
    }
}
```


*** 

Over！










































