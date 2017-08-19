---
title: Leetcode_252
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-26 22:21:00
---
Meeting Rooms

<!-- more -->

***

### Problem
Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), determine if a person could attend all meetings.

For example,
Given [[0, 30],[5, 10],[15, 20]],
return false.

开会时间 不能重叠

### Solution 

{% codeblock lang:java  %}
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public boolean canAttendMeetings(Interval[] intervals) {
    Arrays.sort(intervals, new Comparator<Interval>(){
       public int compare(Interval o1, Interval o2) {
           return o1.start - o2.start;
       } 
    });
    
    for(int i = 1; i < intervals.length; i++) {
        if(intervals[i].start < intervals[i - 1].end) {
            return false;
        }
    }
    return true;
}
{% endcodeblock %}

*** 

Over！










































