---
title: Leetcode_103
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2017-03-24 13:00:00
---
Binary Tree Zigzag Level Order Traversal
层次遍历 变形
<!-- more -->

***

### Problem
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

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
	[20,9],
	[15,7]
]
</pre>

就是奇数行是正向的
偶数行是反向的

### Solution 
精髓：
1. queue 队列！
2. queue.size

``` java
public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        // 加一个 flag 来判断奇数行还是偶数行
        boolean flag = true;
        
        if(root == null) {
            return res;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        while(!queue.isEmpty()) {
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
            if(flag) {
                res.add(list);
            } else {
            	// 偶数行就是reverse一下
                Collections.reverse(list);
                res.add(list);
            }
            flag = !flag;
            
        }
        
        return res;
    }
}
```

*** 

Over！










































