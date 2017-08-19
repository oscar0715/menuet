---
title: Leetcode_513
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-27 15:23:00
---
Find Bottom Left Tree Value

<!-- more -->

***

# Problem
找二叉树最后一行最左边那个节点。

# Solution BFS

层次遍历


``` java
public class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        
        queue.add(root);
        
        int res = root.val;
        
        while(!queue.isEmpty()) {
            boolean flag = false;
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                
                if(!flag){
                    if(node.left==null && node.right==null){
                        res = node.val;
                        flag = true;
                    }
                }
                
                if(node.left!=null) {
                    queue.add(node.left);
                }
                
                if(node.right!=null) {
                    queue.add(node.right);
                }
            }
        }
        
        return res;
    }
}
```

# Solution DFS


``` java
public class Solution {
    public int findBottomLeftValue(TreeNode root) {
        int[] res = new int[]{0,0};
        
        res = dfs(root, 1, res);
        
        return res[0];
    }
    
    private int[] dfs(TreeNode root, int depth, int[]res) {
        
        if(res[1] < depth) {
            // 第一次到这个高度
            // 也就是这一层的最左边的那个
            res[0] = root.val;
            res[1] = depth;
        }
        
        if(root.left != null) {
            dfs(root.left, depth + 1, res); 
        }
        
        if(root.right != null) {
            dfs(root.right, depth + 1, res); 
        }
        
        return res;
    }
}
```

