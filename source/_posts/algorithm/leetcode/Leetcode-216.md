---
title: Leetcode_216
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 23:32:00
---
Combination Sum III
<!-- more -->

***

### Problem
Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

Example 1:
1. Input: k = 3, n = 7
2. Output:
    `[[1,2,4]]`

Example 2:
1. Input: k = 3, n = 9
2. Output:
    `[[1,2,6], [1,3,5], [2,3,4]]`


### Solution
基本的 Backtracking

``` java
public class Solution {
    
    List<List<Integer>> res = new ArrayList<>();
    
    public List<List<Integer>> combinationSum3(int k, int n) {
        
        backtrack(1, n, new ArrayList<>(),k);
        return res;
    }
    
    public void  backtrack(int start, int target, List<Integer> list, int k) {
        if(target == 0 && list.size() == k) {
            res.add(new ArrayList<>(list));
            return;
        } else if (target == 0 || list.size() > k) {
            return;
        }
        
        for(int i = start; i < 10; i++) {
            list.add(i);
            backtrack(i + 1, target - i, list, k);
            list.remove(list.size()-1);
        }
    }
}
```


*** 

Over！










































