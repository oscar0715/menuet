---
title: Leetcode_108
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-08 11:18:00
---
Convert Sorted Array to Binary Search Tree
数组转平衡二叉查找树
<!-- more -->

***

### Problem
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.


### Solution 
1. 数组最中间的为根节点
2. 根节点中间的是左子树
3. 根节点右边的是右子树
4. 递归

{% codeblock lang:java  %}
public TreeNode sortedArrayToBST(int[] nums) {
  if (nums.length == 0) {
    return null;
  }

  TreeNode root = new TreeNode(nums[nums.length/2]);
  root.left = sortedArrayToBST(Arrays.copyOfRange(nums,0, nums.length/2));
  root.right = sortedArrayToBST(Arrays.copyOfRange(nums,nums.length/2+1,nums.length));
  return root;
}
{% endcodeblock %}

这个方法很慢啊，因为要创建新的数组吧

于是用同一个数组，稍微快一点
{% codeblock lang:java  %}
public TreeNode sortedArrayToBST(int[] num) {
  if (num.length == 0)
    return null;
 
  return sortedArrayToBST(num, 0, num.length - 1);
}

public TreeNode sortedArrayToBST(int[] num, int start, int end) {
    if(start > end) {
        return null;
    }
    
    TreeNode root = new TreeNode(num[(start + end) / 2]);
    
    root.left = sortedArrayToBST(num, start, (start + end) / 2 - 1);
    root.right = sortedArrayToBST(num, (start + end) / 2 + 1, end);
    return root;
}
{% endcodeblock %}

*** 

Over！










































