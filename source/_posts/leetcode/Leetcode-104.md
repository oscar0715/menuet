---
title: Leetcode_104
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-08 09:06:00
---
Maximum Depth of Binary Tree
二叉树的深度
<!-- more -->

***

### Problem
Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.


### Solution 
递归

{% codeblock lang:java  %}
public int maxDepth(TreeNode root) {
  if (root == null) {
    return 0;
  }
  int leftDepth = maxDepth(root.left);
  int rightDepth = maxDepth(root.right);
  return Math.max(leftDepth, rightDepth) + 1;
}
{% endcodeblock %}

*** 

Over！










































