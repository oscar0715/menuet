---
title: Leetcode_278
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-01 12:08:00
---
First Bad Version

<!-- more -->

***

# Problem
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

二分查找

# Solution 

二分查找

``` java
public int firstBadVersion(int n) {
    int left = 1;
    int right = n;
    
    while(left < right) {
        // 为什么去掉这个if 反而会快很多
        // if(isBadVersion(left)) {
        //     return left;
        // }   
        
        int middle = left + ((right - left) >> 1);
        
        if(isBadVersion(middle)) {
            right = middle;
        } else {
            left = middle + 1;
        }
    }
    
    return left;
}
```



*** 

Over！










































