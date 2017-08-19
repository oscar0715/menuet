---
title: Leetcode_100
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-05 16:13:00
---
Same Tree
二叉树了！
<!-- more -->

***

### Problem
Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

### Solution 
判断，两棵二叉树是否完全相等
递归
{% codeblock lang:java  %}
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) {
        return true;
    } else if (p == null || q == null) {
        return false;
    }
    return q.val == p.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
{% endcodeblock %}
*** 

Over！










































