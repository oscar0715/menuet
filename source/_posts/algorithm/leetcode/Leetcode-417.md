---
title: Leetcode_414
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-28 19:40:00
---
Pacific Atlantic Water Flow

<!-- more -->

***

### Problem
Example:

Given the following 5x5 matrix:

  Pacific ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic

Return:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).

### Solution 
DFS
1. 分别用两个数组来判断，每个节点是否能流向 Pacific 或者 Atlantic

``` java
public class Solution {
    public List<int[]> pacificAtlantic(int[][] matrix) {
        List<int[]> res = new ArrayList<>();
        
        if(matrix.length == 0){
            return res;
        }
        
        int n = matrix.length;
        int m = matrix[0].length;
        boolean[][] pacific = new boolean[n][m];
        boolean[][] atlantic = new boolean[n][m];

        // Pacific 是左上方
        // Atlantic 是右下方
        for(int i = 0; i < n; i++) {
        	// 最左边的一列对 Pacific DFS
            dfs(matrix, i, 0, Integer.MIN_VALUE, pacific);
            // 最右边的一列对 Atlantic DFS
            dfs(matrix, i, m - 1,  Integer.MIN_VALUE, atlantic);
        }
        
        
        for(int i = 0; i < m; i++) {
        	// 最上面的一行对 Pacific DFS
            dfs(matrix, 0, i, Integer.MIN_VALUE, pacific);
            // 最下面的一行对 Atlantic DFS
            dfs(matrix, n-1, i, Integer.MIN_VALUE, atlantic);
        }
        
        
        for(int i = 0; i < matrix.length; i++) {
            for(int j = 0; j < matrix[i].length; j++) {
            	// 如果对于两片大洋，这个点都是true
            	// 那么就满足条件
                if(pacific[i][j] && atlantic[i][j]) {
                    res.add(new int[]{i,j});
                }
            }
        }
        return res;
        
    }
    
    // DFS 函数
    public void dfs(int[][] matrix, int x, int y, int val, boolean[][] visited) {
        if(!isValid(matrix,x,y) || visited[x][y] || matrix[x][y] < val) {
        	// 如果 x y invalid 那么表示已经抵达大海了
        	// 如果 visited[x][y] == true 那么这个点之前的都判断过了 而且都是 true 的 
        	// 如果 matrix[x][y] < val 那么表示，流不过去了，这时候还是 false
            return;
        }
        
        // 跳出 if 如果没有 return 那么证明是 matrix[x][y] > val 也就是可以继续流动的
        visited[x][y] = true;
        
        // 往四个方向DFS
        dfs(matrix, x - 1, y, matrix[x][y], visited );
        dfs(matrix, x + 1, y, matrix[x][y], visited );
        dfs(matrix, x , y - 1, matrix[x][y], visited );
        dfs(matrix, x , y + 1, matrix[x][y], visited );
       
    }
    
    private boolean isValid(int[][] matrix, int x, int y) {
        if(x < 0 || x >= matrix.length || y < 0 || y >= matrix[x].length) {
            return false;
        }
        return true;
    }
}
```











































