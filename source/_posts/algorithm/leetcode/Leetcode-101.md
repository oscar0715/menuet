---
title: Leetcode_101
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-06 15:52:00
---
Symmetric Tree
二叉树
<!-- more -->

***

### Problem
Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

### Solution 
判断，两棵二叉树是否完全相等
递归
``` java
public boolean isSymmetric(TreeNode root) {
	if (root == null) {
		return true;
	}

	return isSymmetric(root.left, root.right);
}

public boolean isSymmetric(TreeNode p, TreeNode q) {
	if(p == null && q == null) {
		return true;
	} else if(p == null || q == null){
		return false;
	}
	
	return p.val == q.val && isSymmetric(p.left, q.right) && isSymmetric(p.right, q.left);
	
}
```

*** 

Over！










































