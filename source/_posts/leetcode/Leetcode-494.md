---
title: Leetcode_494
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-22 18:49:00
---
Target Sum

<!-- more -->

***

### Problem
给定一个数组，一个整数 S。这列数组每个数字前可以加 '+' 或者 '-'，然后求可以使最后的结果等于 S 的组合方式。

### Solution DFS

DFS 的方法

``` java
public class Solution {
    int count = 0;
    public int findTargetSumWays(int[] nums, int S) {
        
        findTargetSumWays(nums, S, 0);
        return count;
    }
    
    public void findTargetSumWays(int[] nums, int S, int head) {
        if(head == (nums.length-1) ){
            // 这里要注意，如果最后是零的话，就要加两遍
            // 所以两个 if 要分开
            if(nums[head] == S) {
                count++;
            }
            
            if( nums[head] == -S) {
                count++;
            }
        } else {
            findTargetSumWays(nums, S - nums[head], head + 1 );
            findTargetSumWays(nums, S + nums[head], head + 1 );
        }
    }
}
```











































