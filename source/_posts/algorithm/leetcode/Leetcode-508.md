---
title: Leetcode_508
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-29 23:10:00
---
Most Frequent Subtree Sum

<!-- more -->

***

# Problem
求所有子二叉树的和，并且找出出现次数最多的那个和。也就是和的众数。

# Solution 



``` java
public class Solution {
    Map<Integer, Integer> map = new HashMap<>();
    
    public int[] findFrequentTreeSum(TreeNode root) {
         List<Integer> res = new ArrayList<>();
         
         getSum(root);
         
         // 找出众数
         int max = 1;
         for(int num : map.keySet()){
             if(map.get(num) == max) {
                 res.add(num);
             } else if (map.get(num) > max) {
                 res.clear();
                 max = map.get(num);
                 res.add(num);
             }
         }
         
         // List 转 Array
         int[] array = new int[res.size()];
         for(int i = 0; i < res.size(); i++) {
             array[i] = res.get(i);
         }
         
         return array;
    }
    
    // BFS 求合
    private int getSum(TreeNode root) {
        if(root == null) {
            return 0;
        }
        
        int left = 0;
        if(root.left != null) {
            left = getSum(root.left);
        }
        
        int right = 0;
        if(root.right != null) {
            right = getSum(root.right);
        }
        
        int sum = root.val + left + right;
        
        map.put(sum, map.getOrDefault(sum, 0) + 1);
        
        return sum;
    } 
}
```


