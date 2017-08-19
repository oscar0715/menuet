---
title: Leetcode_257
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-31 13:14:00
---
Binary Tree Paths

<!-- more -->

***

### Problem
Given a binary tree, return all root-to-leaf paths.

求二叉树的所有路径

### Solution 
递归嘛

递归真的是很神奇啊

{% codeblock lang:java  %}
public class Solution {
    List<String> result = new ArrayList<String>();

    public List<String> binaryTreePaths(TreeNode root) {
        if (root != null) {
            String path = "";
            addPath(root, path);
        }

        return result;

    }

    private void addPath(TreeNode node, String path) {
        path += node.val;
        // 如果是叶子节点，直接加到list里面
        if (node.left == null && node.right == null) {
            result.add(path);
        }

        path += "->";
        // 如果左子树非空，走左子树
        // 注意进入到递归以后，在那个递归里，path += node.val以后是新建一个String
        // 所以对下面个判断没有影响
        if (node.left != null) {
            addPath(node.left, path);
        }

        // 右子树同理，与上面那一支走向两个一个方向。
        if (node.right != null) {
            addPath(node.right, path);
        }
    }
}
{% endcodeblock %}

*** 

Over！










































