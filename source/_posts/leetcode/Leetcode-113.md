---
title: Leetcode_113
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-08 22:08:00
---
Path Sum II
这个难一点是要把所有路径输出

<!-- more -->

***

### Problem

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,
<pre>
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
</pre>
return 
  [
     [5,4,11,2],
     [5,8,4,5]
  ]

输出所有和为sum的根到叶路径

### Solution 递归思路

用到了输出所有路径那一题的思想。
**<a href="/leetcode/Leetcode-257/">Leetcode-257</a>** : 输出二叉树所有路径

{% codeblock lang:java %}
public class Solution {
List<List<Integer>> result = new ArrayList<List<Integer>>();

  public List<List<Integer>> pathSum(TreeNode root, int sum) {
    if (root == null) {
      return result;
    }
    List<Integer> list = new ArrayList<Integer>();
    pathSum(root, sum, list);
    return result;
  }

  private void pathSum(TreeNode root, int sum, List<Integer> list) {
    if (root == null) {
      return;
    }

    list.add(root.val);
    List<Integer> list2 = new ArrayList<Integer>(list);

    if (root.left == null && root.right == null) {
      if (sum == root.val) {
        result.add(list);
      }
    }

    if (root.left != null) {
      pathSum(root.left, sum - root.val, list);
    }

    if (root.right != null) {
      pathSum(root.right, sum - root.val, list2);
    }
  }
}
{% endcodeblock %}

*** 

Over！










































