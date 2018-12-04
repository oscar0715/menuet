---
title: Leetcode_241
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-21 14:07:00
---
Different Ways to Add Parentheses

<!-- more -->

***

# Problem
给定一个算式，可以任意加小括号，给出所有的结果集

# Solution 
Divide and Conquer
``` java
public class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> res = new ArrayList<>();
        
        for(int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if(c == '+' || c == '-' || c == '*') {
                String part1 = input.substring(0, i);
                String part2 = input.substring(i+1);
                
                // divide
                List<Integer> part1Res = diffWaysToCompute(part1);
                List<Integer> part2Res = diffWaysToCompute(part2);
                
                for(int p1 : part1Res) {
                    for(int p2 : part2Res) {
                        if(c == '+') {
                            res.add(p1 + p2);
                        }
                        if(c == '-') {
                            res.add(p1 - p2);
                        }
                        if(c == '*') {
                            res.add(p1 * p2);
                        }
                    }
                }
            }
        }
        
        // This is the base situation
        if(res.size() == 0) {
            res.add(Integer.parseInt(input));
        }
        
        return res;
    }
}
```

*** 

Over！










































