---
title: Leetcode_114
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-10 18:44:00
---
Flatten Binary Tree to Linked List

<!-- more -->

***

### Problem
Given a binary tree, flatten it to a linked list in-place.

For example,
Given


<pre>
	 1
	/ \
   2   5
  / \   \
 3   4   6
</pre>

<pre>
   1
	\
	 2
	  \
	   3
		\
		 4
		  \
		   5
			\
			 6
</pre>


### Solution 递归思路
1. 先序遍历放进一个queue
2. 然后按找要求排好就好了

{% codeblock lang:java %}
public void flatten(TreeNode root) {
	if(root == null) {
		return;
	}
	
	Stack<TreeNode> stack = new Stack<TreeNode>();
	Queue<TreeNode> queue = new LinkedList<TreeNode>();
	
	stack.push(root);

	while(!stack.isEmpty() ) {
		
		TreeNode node = stack.pop();
		queue.add(node);
		
		if(node.right!=null) {
			stack.push(node.right);
		}
		
		if(node.left!=null) {
			stack.push(node.left);
		}
		
	}
	
	root = null;
	root = queue.poll();
	TreeNode node = root;
	
	while(!queue.isEmpty()){
		node.right = queue.poll();
		node.left = null;
		node = node.right;
	}
}
{% endcodeblock %}

*** 

Over！










































