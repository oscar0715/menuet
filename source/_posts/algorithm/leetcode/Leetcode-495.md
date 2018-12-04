---
title: Leetcode_495
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-04 15:07:00
---
Teemo Attacking

<!-- more -->

***

### Problem
给定一个数组，数组记录的是魔法攻击时间点，另给定一个每次魔法攻击的长度

求敌人受到魔法攻击的总时长

### Solution

``` java
public class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        if(timeSeries.length == 0) {
            return 0;
        }
        int count = 0;
        
        for(int i = 1; i < timeSeries.length; i++) {
            count += Math.min(timeSeries[i] - timeSeries[i-1] , duration);
        }
        count += duration;
        
        return count;
    }
}
```











































