---
title: Leetcode_064
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-31 16:00:00
---
64 Minimum Path Sum
<!-- more -->

***

### Problem
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

给一个二维数组
求从左上角走到右下角的最短路径


### Solution
1. 第0行，第0列累加
2. 之后的 `grid[i][j]` 累加上Math.min(grid[i][j-1], grid[i - 1][j])
3. 最后一格就是最小值


代码如下
``` java
public int minPathSum(int[][] grid) {
	int length = grid.length;
	int width = grid[0].length;

	int i = 0;
	int j = 0;

	for ( i = 0, j = 1; j < width; j++ ) {
		grid[i][j] += grid[i][j-1];
	}
	for ( i = 1, j = 0; i < length; i++ ) {
		grid[i][j] += grid[i - 1][j];
	}

	for ( i = 1; i < length; i++) {
		for (j = 1; j< width; j++){
			grid[i][j] += Math.min(grid[i][j-1], grid[i - 1][j]);
		}
	}

	return grid[length-1][width-1];
}
``` 

*** 

Over！










































