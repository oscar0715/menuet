---
title: Leetcode_198
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-16 13:21:00
---
House Robber

<!-- more -->

***

### Problem
Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

For example:
Given the following binary tree,
<pre>
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
</pre>>

You should return [1, 3, 4].

### Solution 1
1. 层次遍历 
2. BFS

``` java
public List<Integer> rightSideView(TreeNode root) {
	Queue<TreeNode> queue = new LinkedList<TreeNode>();
	List<Integer> result = new ArrayList<Integer>();
	
	if(root == null) {
		return result;
	}
	
	queue.add(root);
	
	while(queue.size() != 0) {
		int size = queue.size();
		int i = 0;
		while(i < size){
			TreeNode node = queue.poll();
			if(node.left != null){
				queue.add(node.left);
			}
			
			if(node.right != null) {
				queue.add(node.right);
			}
			if(i == size - 1) {
				result.add(node.val);
			}
			i++;
		}
	}
	
	return result;
}
```

# Solution 2
1. DFS

``` java
public class Solution {
	public List<Integer> rightSideView(TreeNode root) {
		List<Integer> res = new ArrayList<>();
		
		rightView(root, res, 0);
		
		return res;
	}
	
	public void rightView(TreeNode node, List<Integer> res, int depth) {
		if(node == null) {
			return;
		}
		
		// 这个判断条件就是
		// 把这一层遍历到的第一个数加进来。
		if(depth == res.size()) {
			res.add(node.val);
		}
		
		// 总是先向右走到底
		// 所以最右边的那个总是最先被加到res里的
		// 如果右子树是空的
		// 那么弹出栈顶的这个方法，即访问左子树
		rightView(node.right, res, depth + 1);
		rightView(node.left, res, depth + 1);
		
	}
}
```

*** 

Over！










































