---
title: Leetcode_129
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 18:32:00
---
Sum Root to Leaf Numbers
<!-- more -->

***

### Problem
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

    1
   / \
  2   3
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.


### Solution 

DFS

``` java
public class Solution {
    Integer sum = 0;
    public int sumNumbers(TreeNode root) {
        if(root == null) {
            return 0;
        }
        add(root, 0);
        
        return sum;
    }
    
    public void add(TreeNode root, int curr) {
        curr = curr * 10 + root.val;
        
        if(root.left == null && root.right == null) {
            sum += curr;
            return;
        } 
        
        if (root.left != null) {
            add(root.left, curr);
        } 
        
        if(root.right != null) {
            add(root.right, curr);
        }
    }
}
```

*** 

Over！










































