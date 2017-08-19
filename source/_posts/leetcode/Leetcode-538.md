---
title: Leetcode_538
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-02 21:53:00
---
Convert BST to Greater Tree

<!-- more -->

***

# Problem
给出一个 BST , 每个节点都加上那些自己节点大的所有节点的值

# Solution 
BST 的先序遍历是从小到大。

那么先一直访问右边的就是从大到小。
只要一直加上前一个比自己大的数的更新以后的数值就行了
``` java
public class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        
        dfs(root);
        return root;
    }
    
    private void dfs(TreeNode cur) {
        if(cur == null) {
            return ;
        }
        // 先一直向右访问
        // 找到最大的数
        dfs(cur.right);
        
        // 右节点加上，比自己大的数的合
        cur.val += sum;
        // 更新sum为当前节点的数值
        // 也就是所有大于等于自己的节点的合
        // 供下一个较大节点来加上
        sum = cur.val;
        
        dfs(cur.left);
    }
}
```


