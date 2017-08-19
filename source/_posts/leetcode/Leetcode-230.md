---
title: Leetcode_232
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-01 11:29:00
---
230 Kth Smallest Element in a BST

<!-- more -->

***

### Problem
Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note: 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

Follow up:
    What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

### Solution 

从小到大遍历BST咯
{% codeblock lang:java  %}
public int kthSmallest(TreeNode root, int k) {
    TreeNode node = root;
    
    Stack<TreeNode> stack = new Stack<TreeNode>();
    
    int i = 1;
    
    while(stack != null || node != null) {
        
        while(node != null) {
            stack.push(node);
            node = node.left;
        }
        
        if(stack.empty()) {
            break;
        }
        
        node = stack.pop();
        if( i == k) {
            return node.val;
        }
        i++;
        
        node = node.right;
    }
    
    return 0;
}
{% endcodeblock %}



*** 

Over！










































