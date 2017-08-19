---
title: Leetcode_543
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-22 05:53:00
---
Diameter of Binary Tree

<!-- more -->

***

# Problem
找出这棵二叉树中，任意两点间的最长路径，无所谓经不经过root.

但是本质上肯定是要经过一个root的。

# Solution 

学习到了新知识：
就是可以在递归的同时，去更新一个全局变量，更新动作独立于用于递归的返回值。

``` java
public class Solution {
    int max = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        if(root == null) {
            return 0;
        }
        
        getHeight(root);
        
        return max;
        
    }
    
    public int getHeight(TreeNode root) {
        if(root == null) {
            return 0;
        }
        
        int left = getHeight(root.left);
        int right = getHeight(root.right);
        
        max = Math.max(left + right, max);
        
        return Math.max(left, right) + 1;
    }
}
```


