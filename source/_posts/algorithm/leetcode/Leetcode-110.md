---
title: Leetcode_110
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-08 15:08:00
---
Balanced Binary Tree
判断二叉树是否是平衡二叉树
<!-- more -->

***

### Problem
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

### Solution 思路1
利用了求数组深度的函数

{% codeblock lang:java  %}
public boolean isBalanced(TreeNode root) {
  if (root == null) {
    return true;
  }
  // 求深度的以后，判断放在主函数里
  if (Math.abs(depth(root.left) - depth(root.right)) > 1) {
    return false;
  }
  // 递归！
  return isBalanced(root.left)&&isBalanced(root.right);
}

private int depth(TreeNode p) {
  if (p == null) {
    return 0;
  }
  int left = depth(p.left);
  int right = depth(p.right);
  return Math.max(left, right) + 1;
}

{% endcodeblock %}

### Solution 思路2
判断也放在求深度的函数里。

这个能到1ms哦
{% codeblock lang:java %}
// 主函数
public boolean isBalanced(TreeNode root) {
  return findDepth(root)==-1 ? false : true;
}

private int findDepth(TreeNode p) {
  if (p == null) {
    return 0;
  }

  // 1. 当前节点的左子树不是平衡树
  int leftDepth = findDepth(p.left);
  if (leftDepth == -1) {
    return -1;
  }
  
  // 2. 当前节点的右子树不是平衡树
  int rightDepth = findDepth(p.right);
  if (rightDepth == -1) {
    return -1;
  }
  
  // 3. 当前节点的左右子树各自都是平衡树，但是当前节点的左右不平衡了
  if (leftDepth - rightDepth > 1 || rightDepth - leftDepth > 1) {
    return -1;
  }
  
  // 当前节点是平衡树，继续求深度
  return (leftDepth > rightDepth ? leftDepth : rightDepth) + 1;
}

{% endcodeblock %}
*** 

Over！










































