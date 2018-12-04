---
title: Leetcode_235
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-26 16:38:00
---
Lowest Common Ancestor of a Binary Search Tree

<!-- more -->

***

### Problem

找两个节点在二叉查找树中的Lowest Common Ancestor

### Solution 

关于二叉查找树
**<a href="/ucb61b/lecture25-BinarySearchTree/">UCB 61B Lecture-25</a>**

思路
1. 从根节点开始找，找到的第一个节点值在p,q值中间的就是答案
2. 如果当前节点，比p,q都大，那么到左子树去找一个更小的值
3. 如果当前节点，比p,q都小，那么到右子树去找一个更大的值

{% codeblock lang:java  %}

public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) {
        return null;
    }
    if (root.val >= p.val && root.val >= q.val) {
        return lowestCommonAncestor(root.left, p, q);
    }
    if (root.val <= q.val && root.val <= p.val) {
        return lowestCommonAncestor(root.right, p, q);
    }

    return root;

}
{% endcodeblock %}

*** 

Over！










































