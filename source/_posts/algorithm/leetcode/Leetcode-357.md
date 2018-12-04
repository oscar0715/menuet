---
title: Leetcode_357
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-27 10:02:00
---
Count Numbers with Unique Digits

<!-- more -->

***

### Problem
给定一个n，数出 0 <0 x < 10^n 的所有数字中，所有位都不相同的数字的数量

### Solution Backtrack
基本的回溯
关键是第一位 0 用过以后，就要跳过。
``` java
public class Solution {
    int count = 0;
    public int countNumbersWithUniqueDigits(int n) {
        if(n == 0){
            return 1;
        }
        
        boolean[] flag = new boolean[10];

        backtrack(flag,0, n);
        return count;
    }

    public void backtrack(boolean[] flag, int index, int n) {
        if(index > 0) {
            count++;
        } 
        
        if(index == n) {
            return;
        }
        
        for(int i = 0; i <=9 ; i++) {
            if(flag[0] && index == 1) {
                // 如果第一位是0， 就不能在继续跑下去了。
                continue;
            }
            if(!flag[i]) {
                flag[i] = true;
                
                backtrack(flag, index + 1, n);
                flag[i] = false;
            }
        }
    }
}
```




*** 

Over！










































