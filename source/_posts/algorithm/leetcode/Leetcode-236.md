---
title: Leetcode_236
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-01 19:28:00
---
Lowest Common Ancestor of a Binary Tree

<!-- more -->

***

### Problem

找两个节点在二叉查找树中的Lowest Common Ancestor

### Solution 

普通二叉树的LCA

深度优先算法，递归思想

参考链接:
{% link LCA https://segmentfault.com/a/1190000003509399 LCA %}

{% codeblock lang:java  %}
public class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
      if(root == null || root == p || root == q) {
          return root;
      }
      
      TreeNode left = lowestCommonAncestor(root.left, p, q);
      TreeNode right = lowestCommonAncestor(root.right, p, q);
      
      if (left != null && right != null) {
          return root;
      }
      
      if(left == null) {
          return right;
      } else {
          return left;
      }
      
  }
}
{% endcodeblock %}

*** 

Over！










































