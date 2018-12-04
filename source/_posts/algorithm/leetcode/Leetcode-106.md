---
title: Leetcode_106
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 11:42:00
---
Construct Binary Tree from Inorder and Postorder Traversal
<!-- more -->

***

### Problem
根据中序遍历和后序遍历的结果，构造二叉树

### Solution 
递归

1. 后序遍历用来确定根节点，
2. 中序遍历用来确定左右子树各自的长度。


``` java
public class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        return constructTree(inorder, 0, inorder.length - 1, postorder, 0 , postorder.length - 1);
    }
    
    public TreeNode constructTree(int[] inorder, int inStart, int inEnd, int[] postorder, int postStart, int postEnd) {
        if(inStart > inEnd || postStart > postEnd) {
            return null;
        }
        
        int val = postorder[postEnd];
        TreeNode p = new TreeNode(val);
        
        int k = 0;
        
        for(int i = 0; i < inorder.length; i++) {
            if(inorder[i] == val) {
                k = i;
                break;
            }
        }
        
        p.left = constructTree(inorder, inStart, k - 1, postorder, postStart, postStart + k - 1 - inStart);
        p.right = constructTree(inorder, k + 1, inEnd, postorder, postStart + k - inStart , postEnd - 1);
        
        return p;
    }
}
```

*** 

Over！










































