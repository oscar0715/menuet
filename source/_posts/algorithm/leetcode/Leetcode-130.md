title: Leetcode_130
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 21:32:00
---
Surrounded Regions
<!-- more -->

***

# Problem
给定一个二维字符数组，包含 'X' 和 'O'
要求，把没有连到边界的'O'都换成'X'

# Solution 
先来一个stack overflow的方法

Flood Fill
泛洪填充，简单粗暴的BFS

``` java
public class Solution {
    public void solve(char[][] board) {
        if(board.length < 3 || board[0].length < 3) {
            return;
        }
        // 从四边出发，把和边缘相连的 'O' 都变成 'Y'
        fillBorders(board, 'O', 'Y');
        // 剩下的那些 'O' 就是不和边缘相连的
        // 换成 'X'
        replace(board, 'O', 'X');
        // 把剩下的所有'Y' 换回 'O'
        fillBorders(board, 'Y', 'O');
    }
    
    // 从边界出发
    private void fillBorders(char[][] board, char target, char c) {
        int m = board.length;
        int n = board[0].length;
        
        for(int i = 0; i < m; i++) {
            if(board[i][0] == target) {
                fill(board, i, 0, target, c);
            }
            if(board[i][n-1] == target) {
                fill(board, i, n - 1, target, c);
            }
        }
        
        for(int j = 1; j < n-1; j++) {
            if(board[0][j] == target) {
                fill(board, 0, j, target, c);
            }
            if(board[m-1][j] == target) {
                fill(board, m-1, j, target, c);
            }
        }
    }
    
    private void fill(char[][] board, int i, int j, char target, char c) {
        int m = board.length;
        int n = board[0].length;
        
        if(i < 0 || j < 0 || i >= m || j >= n || board[i][j] != target) {
            return;
        }
        
        board[i][j] = c;
        fill(board, i - 1, j, target, c);
        fill(board, i + 1, j, target, c);
        fill(board, i ,j - 1, target, c);
        fill(board, i ,j + 1, target, c);
    }
    
    private void replace(char[][] board, char target, char c) {
        int m = board.length;
        int n = board[0].length;
        
        for(int i = 0; i < m; i++) {
            for( int j = 0; j < n; j++) {
                if ( board[i][j] == target) {
                    board[i][j] = c;
                } 
            }
        }
    }
}
```


不过尝试把 fill() 方法改为用栈迭代就不会 stackoverflow 了
``` java
private void fill(char[][] board, int i, int j, char target, char c) {
    int m = board.length;
    int n = board[0].length;
    
    Stack<int[]> stack = new Stack<>();
    
    stack.add(new int[]{i,j});
    
    while(!stack.isEmpty()) {
        int[] pair = stack.pop();
        i = pair[0];
        j = pair[1];
        
        if(i < 0 || j < 0 || i >= m || j >= n || board[i][j] != target) {
            continue;
        }
        
        board[pair[0]][pair[1]] = c;
        stack.add(new int[]{i+1,j});
        stack.add(new int[]{i-1,j});
        stack.add(new int[]{i,j+1});
        stack.add(new int[]{i,j-1});
    }
}   
```
*** 











































