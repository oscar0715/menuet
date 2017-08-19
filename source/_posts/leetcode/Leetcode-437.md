---
title: Leetcode_437
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-22 18:17:00
---
Path Sum III

<!-- more -->

***

### Problem
给定一个 sum ，计算二叉树中 PathSum 为 sum 的路径数量。
路径不一定以根节点为起点，也不一定以叶节点为终点。
但是不能跨越根节点。


### Solution 
换一个思路，其实就是判断所有子树从根节点出发的是否存在PathSum = sum。

那么用两层DFS
1. 第一层DFS： 遍历访问每一个节点。
2. 第二层DFS： 访问一个节点时，以该节点为根节点做DFS，求从该节点出发的 PathSum。


``` java
public class Solution {
    int count = 0;
    
    public int pathSum(TreeNode root, int sum) {
        if(root == null) {
            return 0;
        }
        
        // 外层dfs
        // 每个节点都会经历一次内层dfs
        pathSum(root.left, sum);
        pathSum(root.right, sum);
        
        dfs(root, sum);
        
        return count;
    }
    
    // 计算以当前节点为root的，并且从root出发的path Sum
    public void dfs(TreeNode node, int sum) {
        if(node == null) {
            return;
        }
        
        if(node.val == sum) {
            count++;
        }
        
        // 内层dfs
        dfs(node.left, sum-node.val);
        dfs(node.right, sum-node.val);
    }
}
```











































