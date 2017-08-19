---
title: Leetcode_526
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-22 20:02:00
---
Beautiful Arrangement

<!-- more -->

***

# Problem
给定整数 N
排列 1 到 N， 满足以下两个条件
1. The number at the ith position is divisible by i. （num % position == 0）
2. i is divisible by the number at the ith position. (position % num == 0)

问一共有多少种满足要求的排列

# Solution 

Backtracking 思想

``` java
public class Solution {
    int count = 0;
    public int countArrangement(int N) {
        // flag 的长度设置为 N + 1
        // 方便后面计算
        int[] flag = new int[N+1];
        
        dfs(flag, 1);
        
        return count;
    }
    
    public void dfs( int[] flag, int size){
        // size 代表当前集合的大小+1
        // 也就是下一个元素要赋予的位置
        if ( size == flag.length ) {
            // 如果size刚好等于 N + 1
            // 也就是当前集合大小为 N
            count++;
        } else {
            for(int i = 1; i < flag.length; i++) {
                if(flag[i] != 0) {
                    continue;
                }
                // 满足条件
                if(i % size == 0 || size % i == 0) {
                    flag[i] = size;
                    // 那么向下一个位置进行DFS
                    dfs(flag, size + 1);

                    flag[i] = 0;
                }
            }
        } 
    }
}
```

