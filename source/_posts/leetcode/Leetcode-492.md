---
title: Leetcode_491
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-02 23:38:00
---
Construct the Rectangle

<!-- more -->

***

### Problem
给定一个数字 a ，找到最接近的两个数字，乘积为 a 。

### Solution 

``` java
public class Solution {
    public int[] constructRectangle(int area) {

        int i = (int)Math.sqrt(area);
        
        while(i >= 0) {
            if(area % i == 0) {
                return new int[]{area/i, i};
            }
            i--;
        }
        
        return null;
    }
}
```











































