---
title: Leetcode_200
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-25 22:53:00
---
Number of Islands

<!-- more -->

***

### Problem
给定一个二维字符数组，数组中 '1' 代表陆地，'0'代表海水，如果某块陆地，上下左右都被海水包围，就是一块岛屿。数出岛屿的数量。

例子：
Example 1:

```
11110
11010
11000
00000
```
Answer: 1

Example 2:

```
11000
11000
00100
00011
```
Answer: 3

### Solution
``` java
public class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[i].length; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    // 要保证 count ++ 的一定是一整块岛屿的第一个 1
                    // 数完以后，把相连的 1 都置零，以防止重复数。
                    clearRestOfTheLand(grid, i, j);
                }
            }
        }
        
        return count;
    }
    
    public void clearRestOfTheLand(char[][] grid, int i, int j) {
        if(i < 0 || j < 0 || i >= grid.length || j >= grid[i].length || grid[i][j] == '0') {
            return;
        }
        
        grid[i][j] = '0';
        clearRestOfTheLand(grid, i - 1, j);
        clearRestOfTheLand(grid, i + 1, j);
        clearRestOfTheLand(grid, i, j - 1);
        clearRestOfTheLand(grid, i, j + 1);
    }
}
```


*** 

Over！










































