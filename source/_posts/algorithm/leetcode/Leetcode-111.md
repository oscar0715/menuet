---
title: Leetcode_111
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-08 17:08:00
---
Minimum Depth of Binary Tree

<!-- more -->

***

### Problem

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

找最近的叶子节点，就是左右子节点都是null的那一种

### Solution 非递归方法
利用了层次遍历的思路
判断这一层内的每一个节点，是不是左右子树都为null，如果是，就返回，如果不是，就继续往下一层走

{% codeblock lang:java  %}
public int minDepth(TreeNode root) {
    if (root == null) {
      return 0;
    }

    Queue<TreeNode> queue = new LinkedList<TreeNode>();

    queue.add(root);
    int count = 1;
    while (!queue.isEmpty()) {
      int size = queue.size();
      for (int i = 0; i < size; i++) {
        TreeNode node = queue.poll();
        if (node.left == null && node.right == null) {

          return count;
        }
        if (node.left != null) {
          queue.add(node.left);
        }
        if (node.right != null) {
          queue.add(node.right);
        }
      }

      count++;
    }

    return count;

  }

{% endcodeblock %}

### Solution 递归思路

精彩还是递归精彩

{% codeblock lang:java %}
  public int minDepth(TreeNode root) {
    if (root == null) {
      return 0;
    }

    if (root.left == null || root.right == null) {
      // 其中一个为空时，必须取更长的那一支，才是叶子节点
      return Math.max(minDepth(root.left), minDepth(root.right)) + 1;
    } else {
      return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
  }

{% endcodeblock %}

*** 

Over！










































