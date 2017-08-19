---
title: Leetcode_079
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2017-03-27 13:06:00
---
Word Search

<!-- more -->

***

### Problem
给定一个二维数组，找出其中是否有某个单词，可以从任意位置开始，字母不能重复使用。

For example,
Given board =

```
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```

word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.

### Solution

回溯
1. 带返回值的回溯

``` java
public class Solution {
    
    public boolean exist(char[][] board, String word) {
        boolean res = false;
        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < board[i].length; j++) {
                res = backtrack(board, i, j, word, "");
                if(res) {
                    return res;
                }
            }
        }
 
        return res;
    }
    
    private boolean backtrack(char board[][], int i, int j, String word, String cur) {
        if(!cur.equals(word.substring(0,cur.length()))) {
            return false;
        }
                
                
        if(!isValid(i,j,board)){
            return false;
        }
        
        boolean val = false;
        
        if(board[i][j] != ' ') {
            char c = board[i][j];
            
            String s = cur + c;
            
            if(s.equals(word)){
                return true;
            }
        
            board[i][j] = ' ';
            
            
            val = backtrack(board, i - 1, j, word, s) ||
                          backtrack(board, i + 1, j, word, s) ||
                          backtrack(board, i, j - 1, word, s) ||
                          backtrack(board, i, j + 1, word, s);

            // 这一行不能忘记
            // 要恢复这个位置。
            // 才不会影响在方法栈下面的方法。
            board[i][j] = c;
        } 
        
        return val;
    }
    
    private boolean isValid(int i, int j, char[][] board) {
        if(i < 0 || i >= board.length || j < 0 || j >= board[i].length) {
            return false;
        }
        return true;
    }
}
```


*** 

Over！



















































