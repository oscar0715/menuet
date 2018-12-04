---
title: Leetcode_062
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-30 23:06:00
---
Unique Paths II
<!-- more -->

***

### Problem

给一个二维数组
求从左上角走到右下角的所有可能性

中间有一些路障。


### Solution

代码如下


``` java
public class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        
        // 第一列如果有一个点有路障，后面都是0
        for(int i = 0; i < m; i++) {
            if(obstacleGrid[i][0] == 1) {
                obstacleGrid[i][0] = 0;
            } else {
                if(i-1>=0) {
                    obstacleGrid[i][0] = obstacleGrid[i-1][0];
                } else {
                    obstacleGrid[i][0] = 1;
                }
            }
        }
        
        // 第一行如果有一个点有路障，后面都是0
        // 这里可以从 1 开始
        for(int i = 1; i < n; i++) {
            if(obstacleGrid[0][i] == 1) {
                obstacleGrid[0][i] = 0;
            } else {
                    obstacleGrid[0][i] = obstacleGrid[0][i-1];
            }
        }
        
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                if(obstacleGrid[i][j] == 1) {
                    obstacleGrid[i][j] = 0;
                } else {
                	// 动态规划
                    obstacleGrid[i][j] =  obstacleGrid[i-1][j] + obstacleGrid[i][j-1];
                }
            }
        }

        // 最后要特别判断一下
        if(obstacleGrid[m-1][n-1] == -1) {
            return 0;
        }
        return obstacleGrid[m-1][n-1];
    }
}
```

*** 

Over！










































