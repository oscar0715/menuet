---
title: Leetcode_102
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2016-07-08 00:06:00
---
Binary Tree Level Order Traversal
层次遍历
<!-- more -->

***

### Problem
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree [3,9,20,null,null,15,7],
<pre>    3
	 / \
	9  20
		/  \
	 15   7
</pre>
return its level order traversal as:
<pre>[
	[3],
	[9,20],
	[15,7]
]
</pre>


### Solution 
精髓：
1. queue 队列！
2. queue.size

``` java
public List<List<Integer>> levelOrder(TreeNode root) {
	List<List<Integer>> res = new ArrayList<List<Integer>>();

	if (root == null) {
		return res;
	}

	Queue<TreeNode> queue = new LinkedList<TreeNode>();
	queue.add(root);

	// 在层次遍历的基础上，每次判断 queque.isempty() 后，加一个queue.size()
	// 并用 size 做内层循环条件，这样内层循环，只循环一层的元素
	while (!queue.isEmpty()) {
		int size = queue.size();

		List<Integer> list = new ArrayList<>();
				
		TreeNode node = null;
		while(size > 0) {
				
			node = queue.poll();
			list.add(node.val);
			
			if(node.left!=null) {
				queue.add(node.left);
			}
			
			if(node.right!=null) {
				queue.add(node.right);
			}
			size--;
		}

		res.add(list);
	}
	return res;
}
```

*** 

Over！










































