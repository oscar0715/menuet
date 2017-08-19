---
title: Leetcode_117
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2016-08-05 11:08:00
---
Populating Next Right Pointers in Each Node
任意二叉树的情况

<!-- more -->

***

### Problem

Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

You may only use constant extra space.
For example,
Given the following binary tree,

<pre>
				 1
			 /  \
			2    3
		 / \    \
		4   5    7
</pre>
After calling your function, the tree should look like:
<pre>
				 1 -> NULL
			 /  \
			2 -> 3 -> NULL
		 / \    \
		4-> 5 -> 7 -> NULL
</pre>


### Solution 
1. 基本思路就是走当前这一层，连下一层
2. 先连当前节点的左右子节点
3. 再把当前节点的子节点和前面一个节点的子节点连在一起

``` java
public void connect(TreeLinkNode root) {
	if (root == null) {
		return;
	}

	TreeLinkNode currentHead = root;
	TreeLinkNode nextHead = null;

	while (currentHead != null) {
		TreeLinkNode currentNode = currentHead;

		// Tail是next层中，已经连过的最后一个节点
		TreeLinkNode nextTail = null;

		while (currentNode != null) {
			// 【第一步】：先把currentNode下的左右子节点连接起来
			if (currentNode.left != null && currentNode.right != null) {
				currentNode.left.next = currentNode.right;
			}

			// 找当前节点靠左的那个，用来和下一层中更靠左的节点连接，记为 nextNode
			// nextNode 也就是 currentNode下第一个非空的子节点
			TreeLinkNode nextNode;
			if (currentNode.left != null) {
				nextNode = currentNode.left;
			} else if (currentNode.right != null) {
				nextNode = currentNode.right;
			} else {
				nextNode = null;
			}

			if (nextNode != null) {

				if (nextHead == null) {
					// 如果next层的head还为空
					// 那么nextNode自己就是最靠左的
					// 将nextNode赋到nextHead
					nextHead = nextNode;
					// 同时赋到nextTail
					nextTail = nextNode;
				} else {
					// 非空的话
					// 【第二步】：next层中，把前一个节点nextTail和当前节点nextNode连接起来
					// 也就是和更靠左的那个连接起来
					nextTail.next = nextNode;

				}

				
				while (nextTail.next != null) {
					// 完了以后nextTail指向next层连接过的最靠右的那个节点。
					// 最右边也就是当前节点的右边子节点了
					// 循环是因为，如果currentNode左右节点都在，就需要跳过两个节点
					nextTail = nextTail.next;
				}

			}
			currentNode = currentNode.next;

		}
		currentHead = nextHead;
		nextHead = null;
	}
}
```

*** 

Over！










































