---
title: Leetcode_94
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-08 12:13:00
---
Binary Tree Inorder Traversal
二叉树的中序遍历
<!-- more -->

***

### Problem
Given a binary tree, return the inorder traversal of its nodes' values.

二叉树的中序遍历

### Solution 迭代 

迭代中序遍历的两种写法

{% codeblock lang:java  %}
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();
    Stack<TreeNode> stack = new Stack<TreeNode>();

    if (root == null) {
        return list;
    }

    while (root != null) {
        stack.push(root);
        root = root.left;
    }

    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        list.add(node.val);

        if (node.right != null) {
            node = node.right;
            while (node != null) {
                stack.push(node);
                node = node.left;
            }
        }
    }

    return list;
}
{% endcodeblock %}

{% codeblock lang:java  %}
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();
    Stack<TreeNode> stack = new Stack<TreeNode>();
    while (root != null || !stack.isEmpty()) {
        if (root != null) {
            stack.push(root);
            root = root.left;
        } else {
            root = stack.pop();
            list.add(root.val);
            root = root.right;
        }
    }
    return list;
}
{% endcodeblock %}


### Solution 递归

递归写法：

{% codeblock lang:java  %}
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();
    if (root == null) {
        return list;
    }

    return inorderTraversal(root ,list);
}

private List<Integer> inorderTraversal(TreeNode root, List<Integer> list) {
    if (root==null) {
        return list;
    }
    inorderTraversal(root.left, list);
    list.add(root.val);
    inorderTraversal(root.right, list);

    return list;
}
{% endcodeblock %}

### Solution Morris遍历
常数空间，不用list
{% codeblock lang:java  %}
// 挖个坑
{% endcodeblock %}
*** 

Over！










































