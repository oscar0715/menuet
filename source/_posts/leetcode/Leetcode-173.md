---
title: Leetcode_173
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-31 10:54:00
---
Binary Search Tree Iterator  

<!-- more -->

***

### Problem
mplement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

遍历查找二叉树，按从小到大的顺序

### Solution
1. 用一个stack来存节点，晓得节点在最上面
2. 构造器，先存从根节点到最小节点的路径
3. 每次pop出最小的元素的时候，都再将该节点右子树重复构造器的操作（因为当前节点的右子树，肯定比stack里的下一个元素要小）

本质就是stack，Java里用DeQueue会更快

{% codeblock lang:java  %}
/**
 * Definition for binary tree
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode(int x) { val = x; }
 * }
 */
public class solution173BinarySearchTreeIterator {

    Stack<TreeNode> stack = new Stack<TreeNode>();

    public solution173BinarySearchTreeIterator(TreeNode root) {
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    }

    /**
     * @return whether we have a next smallest number
     */
    public boolean hasNext() {
        return !stack.isEmpty();
    }

    /**
     * @return the next smallest number
     */
    public int next() {
        TreeNode min = stack.pop();
        TreeNode minRight = min.right;
        while (minRight != null) {
            stack.push(minRight);
            minRight = minRight.left;
        }
        return min.val;
    }
}
{% endcodeblock %}


*** 

Over！










































