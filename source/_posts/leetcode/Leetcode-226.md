---
title: Leetcode_226
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-19 13:14:00
---
Invert Binary Tree

<!-- more -->

***

### Problem

Invert a binary tree.
<pre>     
	 4
   /   \
  2     7
 / \   / \
1   3 6   9
</pre>
to
<pre>     
	 4
   /   \
  7     2
 / \   / \
9   6 3   1
</pre>

### Solution 递归

二叉树还是递归比较快

{% codeblock lang:java  %}
public TreeNode invertTree(TreeNode root) {

	System.out.println(levelOrder(root));
	if (root == null) {
		return null;
	}

	TreeNode result = root;

	exchange(root);

	System.out.println(levelOrder(result));
	return  result;

}

private void exchange(TreeNode root) {

	if (root.left == null && root.right == null) {
		return;
	}

	TreeNode tmp = root.left;
	root.left = root.right;
	root.right = tmp;

	if (root.left != null) {
		exchange(root.left);
	}
	if (root.right != null) {
		exchange(root.right);
	}

}
{% endcodeblock %}

### Solution 非递归

其实还是用的层次遍历的思想，比较慢

{% codeblock lang:java  %}
public TreeNode invertTree(TreeNode root) {

	if (root == null) {
		return null;
	}


	TreeNode result = root;

	Deque<TreeNode> queue = new ArrayDeque<TreeNode>();
	queue.add(root);
	while (!queue.isEmpty()){
		TreeNode node = queue.pollFirst();
		exchange(node);
		if (node.left!=null){
			queue.addLast(node.left);
		}
		if (node.right!=null){
			queue.addLast(node.right);
		}
	}

	System.out.println(levelOrder(result));
	return result;

}
 
private void exchange(TreeNode root) {

	if (root.left == null && root.right == null) {
		return;
	}
	
	TreeNode tmp = root.left;
	root.left = root.right;
	root.right = tmp;

}
{% endcodeblock %}

*** 

Over！










































