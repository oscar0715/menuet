---
title: Leetcode_95
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-08 22:41:00
---
Unique Binary Search Trees II
<!-- more -->

***

### Problem
Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1...n.

For example,
Given n = 3, your program should return all 5 unique BST's shown below.

<pre>   
   1         3     3      2      1
	\       /     /      / \      \
	 3     2     1      1   3      2
	/     /       \                 \
   2     1         2                 3
</pre>

### Solution 

别人写的答案，看来我还是没有掌握递归的精髓

{% codeblock lang:java  %}
public List<TreeNode> generateTrees(int n) {
	List<TreeNode> result = new ArrayList<TreeNode>();
	 
	// n 如果小于等于0 就不考虑了 
	if(n > 0) {
		result = genBST(1, n);
	} 

	return result;
}

private List<TreeNode> genBST(int min, int max) {
	List<TreeNode> result = new ArrayList<TreeNode>();
	
	if (min > max) {
		result.add(null);
		return result;
	}
	
	// i <= max, max也可以做root
	for( int i = min ; i <= max; i++) {
		List<TreeNode> leftSubTree = genBST(min, i - 1);
		List<TreeNode> rightSubTree = genBST(i + 1, max);
		
		for( int j = 0; j < leftSubTree.size(); j++) {
			for( int k = 0; k < rightSubTree.size(); k++) {
				TreeNode root = new TreeNode(i);
				root.left = leftSubTree.get(j);
				root.right = rightSubTree.get(k);
				result.add(root);
			}
		}
	 }
	 
	 return result;
}
{% endcodeblock %}

*** 

Over！










































