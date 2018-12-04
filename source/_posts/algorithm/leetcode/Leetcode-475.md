---
title: Leetcode_475
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-27 16:43:00
---
Heaters

<!-- more -->

***

# Problem
给定一个数组 houses 代表房子的坐标（一维坐标）
给定一个数组 heater 代表暖气的坐标（一维）

求所有房子都可以被覆盖到暖气的情况下，暖气的最小半径。

# Solution 1

>利用 Arrays.binarySearch()
// if none, return (-(insertion point) - 1) 
int index = Arrays.binarySearch(heaters, house);

``` java
public class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        
        int res = 0;
        Arrays.sort(heaters);
        
        for(int house : houses) {
            // 对于每一座房子都去找在heater数组中有没有自己的坐标

            // if none, return (-(insertion point) - 1) 
            int index = Arrays.binarySearch(heaters, house);
            
            // 如果没有找房子的坐标返回值是一个负值
            if(index < 0) {
                // 通过公式，找到insertion point
                index = - (index + 1);
            }

            // 求到左侧heater的距离
            int dist1 = index - 1 >= 0 ? house - heaters[index - 1] : Integer.MAX_VALUE;
            // 到右侧heater的距离
            int dist2 = index < heaters.length ? heaters[index] - house : Integer.MAX_VALUE;

            // 用左右距离的更小值，去和res比，取更大值
            res = Math.max(res, Math.min( dist1, dist2 ));
        }
        
        return res;
    }
}
```







































