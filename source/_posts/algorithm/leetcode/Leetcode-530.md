---
title: Leetcode_530
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-02 22:23:00
---
Minimum Absolute Difference in BST

<!-- more -->

***

# Problem
返回一个 BST 任意两数的最小差值

# Solution 

原本想不到怎么比较最小的那个节点，想着用一个 List 存下来
看到别人用一个Integer对象存起来，觉得挺好的。

``` java
public class Solution {
    int diff = Integer.MAX_VALUE;
    // 最小值用一个 Integer 对象保存
    // 这样访问最小值的时候，Integer 就是 null 
    // 这样可以跳过这个case
    Integer min = null;
    public int getMinimumDifference(TreeNode root) {
        
        if(root == null && root.left == null && root.right == null) {
            return 0;
        }
        
        dfs(root);

        
        return diff;
    }
    
    private void dfs(TreeNode root) {
        if(root == null) {
            return;
        }
        dfs(root.left);
        
        if(min != null) {
            diff = Math.min(diff, root.val - min);
        }
        
        min = root.val;
        
        dfs(root.right);
    }
}
```


