---
title: Leetcode_112
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-08 19:08:00
---
Path Sum

<!-- more -->

***

### Problem

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,
<pre>
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
</pre>
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

求是否存在一条从根节点到叶子节点的路径上的值的和等于给定值

### Solution 非递归方法
利用了层次遍历的思路

{% codeblock lang:java  %}
public boolean hasPathSum(TreeNode root, int sum) {

    if (root == null) {
      return false;
    }

    Queue<TreeNode> treeNodes = new LinkedList<TreeNode>();

    treeNodes.add(root);
    while (!treeNodes.isEmpty()) {
      int size = treeNodes.size();
      for (int i = 0; i < size; i++) {
        TreeNode p = treeNodes.poll();
        if (p.val == sum && p.left == null && p.right == null) {
          return true;
        }
        if (p.left != null) {
          p.left.val = p.left.val + p.val;
          treeNodes.add(p.left);
        }
        if (p.right != null) {
          p.right.val = p.right.val + p.val;
          treeNodes.add(p.right);
        }
      }
    }

    return false;
  }

{% endcodeblock %}

### Solution 递归思路

递归更快，递归更漂亮。

我还有什么可以说的

{% codeblock lang:java %}
public boolean hasPathSum(TreeNode root, int sum) {
  if (root == null) {
    return false;
  }
  if (root.left == null && root.right == null && root.val == sum) {
    return true;
  }

  return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
}

{% endcodeblock %}

*** 

Over！










































