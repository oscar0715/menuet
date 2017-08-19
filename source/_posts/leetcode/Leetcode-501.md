---
title: Leetcode_501
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-22 15:28:00
---
Find Mode in Binary Search Tree

<!-- more -->

***

# Problem
找出平衡二叉树中的众数（们）。

平衡二叉树的特别：
1. 左子节点比父节点小或者相等。
2. 右子节点比父节点大或者相等。
3. 左右子树都是平衡二叉树。

# Solution 

平衡二叉树的中序遍历刚好将所有结点从小到大遍历一遍。

1. 中序遍历
2. 用current记录当前用来比较的值
3. 用count记录current出现次数
4. 用maxCount用来记录众数出现次数


``` java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
 
 // 二叉树的中序遍历正好是BST从小到大的排列
 
public class Solution {
    public int[] findMode(TreeNode root) {
        if(root == null) {
            return new int[0];
        }
        List<Integer> list = new ArrayList<>();
        
        TreeNode node = root;
        
        int maxCount = 0;
        int count = 0;
        int current = root.val;
        
        Stack<TreeNode> stack = new Stack<>();
        
        while(node != null || !stack.isEmpty()){
            
            if(node != null){
                // 中序遍历，先找到当前子树最左边的那个
                stack.push(node);
                // 一路向左走
                node = node.left;
            } else {
                // 左边没路了，访问栈里的第一个弹出来的
                // 也就是回头找上一个父节点
                node = stack.pop();
                
                // 找到以后访问它
                // 在这个题目里也就是作比较
                int val = node.val;
                
                if(current == val) {
                    count++;
                } else {
                    current = val;
                    count = 1;
                }
                
                if(count == maxCount) {
                    // 达到了之前众数的count的话
                    // 加到list里面
                    list.add(val);
                } else if(count > maxCount) {
                    // 超过了的话
                    // 前面的找出来的数就都不是众数了
                    maxCount = count;
                    list.clear();
                    list.add(val);
                } 
                
                // 这个节点访问完了
                // 因为左子树已经空了，就到右子树去了
                node = node.right;
            }
        }
        
        // list转int[]
        // 基本类型数组不能用toArray，只能用笨办法
        int[] res = new int[list.size()];
        int i = 0;
        while(i < list.size()){
            res[i] = list.get(i);
            i++;
        }
        return res;
    }
}
```

# Solution Recursive
递归方法快一些，要写一个helper方法，但是因此就要用全局变量了。

每次递归，访问节点的时候，改变全局变量的状态。

``` java
public class Solution {
    
    List<Integer> list = new ArrayList<>();
    int maxCount = 0;
    int count = 0;
    int current = 0;
    
    public int[] findMode(TreeNode root) {
        if(root == null) {
            return new int[0];
        }
        
        traverse(root);
        
        // list转int[]
        // 基本类型数组，不能用toArray，只能用笨办法
        int[] res = new int[list.size()];
        int i = 0;
        while(i < list.size()){
            res[i] = list.get(i);
            i++;
        }
        
        return res;
    }
    
    public void traverse(TreeNode node) {
        if(node == null) {
            return;
        }
        
        traverse(node.left);
        
        // 找到以后访问它
        // 在这个题目里也就是作比较
        int val = node.val;
        
        if(current == val) {
            count++;
        } else {
            current = val;
            count = 1;
        }
        
        if(count == maxCount) {
            // 达到了之前众数的count的话
            // 加到list里面
            list.add(val);
        } else if(count > maxCount) {
            // 超过了的话
            // 前面的找出来的数就都不是众数了
            maxCount = count;
            list.clear();
            list.add(val);
        }   
        
        traverse(node.right);
    }
}
```

