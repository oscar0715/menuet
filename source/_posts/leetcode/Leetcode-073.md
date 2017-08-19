---
title: Leetcode_073
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-04 13:21:00
---
Set Matrix Zeroes
<!-- more -->

***

### Problem
给定一个二维数组，如果某个元素是 0， 那么把这个元素所在的整个列和整个行都变成0


### Solution
基本思路就是
1. 先遍历一遍，如果遇到0，把行首和列首标记为0
2. 第二遍的时候，如果列首或者行首是0的情况，改为0

但是要注意第一列要空出来

``` java
public class Solution {
    public void setZeroes(int[][] matrix) {
        int n = matrix.length;
        int m = matrix[0].length;
        int col0 = 1;

        // mark 
        // 标记行首 列首
        
        for(int i = 0; i < n; i++) {
            if(matrix[i][0] == 0) {
                col0 = 0;
            } 

            // 注意这里从1开始，因为[0][0]这个位置是重叠的，会影响判断
            // 所以我们单独定义一个 col0 这个变量来判断改变第0列
            // [0][0] 只用来判断行
            for( int j = 1; j < m; j++) {
                if(matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }    
            }
        }

        // change column
        // 这里也从1开始
        for(int i = 1; i < matrix[0].length; i++) {
            if(matrix[0][i] == 0) {
                for(int j = 0; j < n; j++){
                    matrix[j][i] = 0;
                }
            }
        }
        
        // change row
        for(int i = 0; i < n; i++) {
            if(matrix[i][0] == 0) {
                for(int j = 0; j < m; j++){
                    matrix[i][j] = 0;
                }
            }
            // 这里用col0
            // 顺带改了第0列
            if(col0 == 0) 
                matrix[i][0] = 0;
        }
    }
}
```

*** 

Over！










































