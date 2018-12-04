---
title: Leetcode_98
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-08 10:13:00
---
Validate Binary Search Tree
<!-- more -->

***

### Problem
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

### Solution 
1. 中序遍历
2. 广度优先搜索
3. 其实就是从小到大遍历二叉搜索树

{% codeblock lang:java  %}
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        Stack<TreeNode> stack = new Stack<TreeNode>();

        TreeNode cur = root;
        TreeNode pre = null;

        while (true) {
            // 每次都找到该分支最小的节点
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }

            // 循环退出条件
            if (stack.isEmpty()) {
                break;
            }
            cur = stack.pop();

            if (pre != null && pre.val >= cur.val) {
                return false;
            }

            pre = cur;
            // cur已经是最小节点了，然后到right里找下一个最小节点
            cur = cur.right;

        }

        return true;
    }
{% endcodeblock %}

*** 

Over！










































