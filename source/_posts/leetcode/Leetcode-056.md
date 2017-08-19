---
title: Leetcode_056
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-05 22:33:00
---
Merge Intervals
<!-- more -->

***

# Problem
合并区间列表

# Solution


``` java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public class Solution {
    public List<Interval> merge(List<Interval> intervals) {
        
        List<Interval> res = new ArrayList<Interval>();
        
        if(intervals.size() == 0) {
            return res;
        }
        
        // 排序
        Collections.sort(intervals, new Comparator<Interval>(){
            public int compare(Interval i1, Interval i2) {
                if(i1.start - i2.start != 0) {
                    return i1.start - i2.start;
                } else {
                    return i1.end - i2.end;
                }
            } 
        });
        
        Interval pre = intervals.get(0);
        for(int i = 1; i < intervals.size(); i++){
            Interval cur = intervals.get(i);
            if(pre.end < cur.start) {
                res.add(pre);
                pre = cur;
            } else {
                pre.end = Math.max(pre.end, cur.end);
            }
        }
        
        res.add(pre);
        
        return res;
    }
}
```

*** 

Over！










































