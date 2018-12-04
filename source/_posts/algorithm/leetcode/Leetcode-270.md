---
title: Leetcode_270
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-01 13:14:00
---
Closest Binary Search Tree Value

<!-- more -->

***

### Problem
Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.



### Solution 
二叉查找树
迭代可以写

{% codeblock lang:java  %}
    public int closestValue(TreeNode root, double target) {
        
        int result = root.val;
        
        while(root != null ) {
            
            if (root.val == target) {
                return root.val;
            }
            
            if(Math.abs(root.val - target) < Math.abs(result - target)) {
                result = root.val;
            } 
            
            if(root.val > target) {
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return result;
    }
{% endcodeblock %}

*** 

Over！










































