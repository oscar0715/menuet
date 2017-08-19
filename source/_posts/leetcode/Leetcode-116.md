---
title: Leetcode_116
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-04 20:08:00
---
Populating Next Right Pointers in Each Node
满二叉树的情况

<!-- more -->

***

### Problem

Given a binary tree

    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
For example,
Given the following perfect binary tree,
<pre>
        1
       /  \
      2    3
     / \  / \
    4  5  6  7
</pre>
After calling your function, the tree should look like:
<pre>
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
</pre>


### Solution 
基本思路就是走当前这一层，连下一层

{% codeblock lang:java  %}
public void connect(TreeLinkNode root) {
  if(root == null) {
      return;
  }

  TreeLinkNode current;
  
  // 遍历current(root)层，指定current层的下一层的next指针
  while (root.left != null) {
    current = root;
    while (current != null) {
      //指定left节点的next
      current.left.next = current.right;
      if (current.next != null) {
        // 若current的next节点不为空
        // 则可指定right节点的next
        current.right.next = current.next.left;
      }
      current = current.next;
    }
    root = root.left;
  } 
}
{% endcodeblock %}

*** 

Over！










































