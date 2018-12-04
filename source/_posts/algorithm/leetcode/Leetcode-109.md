---
title: Leetcode_109
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 13:08:00
---
Convert Sorted List to Binary Search Tree
有序链表转平衡二叉查找树
<!-- more -->

***

### Problem
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.


### Solution 
思想上和数组转 BST 是一模一样的

1. 链表最中间的为根节点
2. 根节点中间的是左子树
3. 根节点右边的是右子树
4. 递归

``` java
public class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null) {
            return null;
        }
        
        return construct(head, null);
    }
    
    public TreeNode construct(ListNode head, ListNode tail) {
        if( head == tail ) {
            return null;
        }
        
        // 找中点
        ListNode slow = head;
        ListNode fast = head;
        
        while( fast != tail && fast.next != tail) {
            fast = fast.next.next;
            slow = slow.next;
        }
        
        TreeNode node = new TreeNode(slow.val);
        
        // 递归
        node.left = construct(head, slow);
        node.right = construct(slow.next, tail);
        
        return node;
    }
}
```

*** 

Over！










































