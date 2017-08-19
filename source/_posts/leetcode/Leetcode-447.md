---
title: Leetcode_447
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-29 15:44:00
---
Number of Boomerangs

<!-- more -->

***

### Problem
给定一个点集。找出能组成的 boomerang 的个数。

boomerang 的定义是一个 (i, j, k) 元组：
满足 distance(i, j) = distance(j, k)

### Solution 

``` java
public class Solution {
    public int numberOfBoomerangs(int[][] points) {
        
        Map<Integer, Integer> map = new HashMap<>();
        
        int res = 0;
        for(int i = 0; i < points.length; i++ ) {
            for(int j = 0; j < points.length; j++ ) {
                if(i == j) continue;
                int d = getDistance(points[i], points[j]);
                map.put( d, map.getOrDefault(map.get(d), 0) + 1 );
            }
            
            // (以外层循环那个点，在第一个位置上，排列第二个位置和第三个位置)
            // 也就是说点 point[i] 为第一个点
            // 从 map.get(point(i)) 中任意取两个点，放在第二个位置和第三个位置进行排列
            for(int val : map.values()) {
                res += val * (val - 1);
            }
            
            // 每跑完一轮就加一轮
            // 加完以后，以这个点为第一个位置的计算就结束了
            // 清空 map, 开始下一轮计算
            map.clear();
        }
        
        return res;
        
    }
    
    private int getDistance(int[] a , int[] b) {
        int x = a[0] - a[0];
        int y = a[1] - b[1];
        return  x*x + y*y;
    }
}
```











































