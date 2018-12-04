---
title: Leetcode_539
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-02 21:01:00
---
Minimum Time Difference

<!-- more -->

***

# Problem
给定一串时间值，返回任意两个时间点最小的时间间隔

Example 1:
```
	Input: ["23:59","00:00"]
	Output: 1
```

# Solution 
第一种方法 用 HashMap
``` java
public class Solution {
    public int findMinDifference(List<String> timePoints) {
    	// 排个序
        Collections.sort(timePoints);
        
        int res = Integer.MAX_VALUE;
        
        for( int i = 1; i < timePoints.size(); i++) {
        	// 前后两个比较
            int diff = getDiff(timePoints.get(i-1), timePoints.get(i));
            res = Math.min(diff, res);
        }
        
        // 第一个和最后一个比较
        // 这个要拆开来看
        // 因为中间隔着 00:00 或者说 24:00
        int val1 = getDiff(timePoints.get(timePoints.size()-1), "24:00");
        int val2 = getDiff("00:00", timePoints.get(0));
        res = Math.min((val1+val2), res);
        
        return res;
    }
    
    private int getDiff(String str1, String str2) {
        
        int h1 = Integer.parseInt(str1.substring(0,2));
        int h2 = Integer.parseInt(str2.substring(0,2));
        
        int m1 = Integer.parseInt(str1.substring(3,5));
        int m2 = Integer.parseInt(str2.substring(3,5));
        
        int diff = 60 * (h2-h1) - m1 + m2;
        
        return  diff;
    }
}
```


