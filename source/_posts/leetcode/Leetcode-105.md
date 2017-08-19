---
title: Leetcode_105
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 11:40:00
---
Construct Binary Tree from Preorder and Inorder Traversal
<!-- more -->

***

### Problem
根据先序遍历和中序遍历的结果，构造二叉树。

### Solution 
1. 先序遍历用来确定根节点，
2. 中序遍历用来确定左右子树各自的长度。

关键是每次递归，找准子树在两个数组中起始位置和终止位置。

``` java
public class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return constructTree(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1 );
    }
    
    public TreeNode constructTree(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
        if(preStart > preEnd || inStart > inEnd){
            return null;
        }
        
        int val = preorder[preStart];
        
        TreeNode p = new TreeNode(val);
        
        int k = 0;
        for(int i = 0; i < inorder.length; i++) {
            if(val == inorder[i]) {
                k = i;
                break;
            }
        }
        
        p.left = constructTree(preorder, preStart + 1, preStart + k - inStart, inorder, inStart, k - 1 );
        p.right = constructTree(preorder, preStart + k - inStart + 1, preEnd, inorder, k + 1, inEnd);
        
        return p;
    }
}
```

*** 

Over！










































