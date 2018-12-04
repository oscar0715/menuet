---
title: Leetcode_062
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-03 12:00:00
---
Unique Paths

与64 Minimum Path Sum 原理相同
<!-- more -->

***

### Problem

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

给一个二维数组
求从左上角走到右下角的所有可能性


### Solution
1. 第0行，第0列可能性都是1
2. 之后的 `grid[i][j] = grid[i][j-1] + grid[i - 1][j]`
3. 最后一格就是result


代码如下
{% codeblock lang:java %}
public int uniquePaths(int m, int n) {
	int[][] grid = new int[m][n];
	
	for(int i = 0; i < m; i++) {
		grid[i][0] = 1;
	}
	
	for(int i = 0; i < n; i++) {
		grid[0][i] = 1;
	}
	
	for(int i = 1; i < m; i++) {
		for(int j = 1; j < n; j++) {
			grid[i][j] = grid[i - 1][j] + grid[i][j - 1];
		}
	}
	
	return grid[m - 1][n - 1];
}
{% endcodeblock %}

*** 

Over！










































