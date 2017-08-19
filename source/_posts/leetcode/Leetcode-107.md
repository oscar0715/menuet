---
title: Leetcode_107
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-08 09:18:00
---
Binary Tree Level Order Traversal II
自下而上的层次遍历
<!-- more -->

***

### Problem
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

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
  [15,7],
  [9,20],
  [3]
]
</pre>


### Solution 
额 这和题目醉醉的。就是自上而下的层次遍历完以后，把list颠倒一下…………………………

{% codeblock lang:java  %}
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> list = new ArrayList<List<Integer>>();
    List<Integer> inResult = new ArrayList<Integer>();

    if (root == null) {
        return list;
    }

    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);

    // 在层次遍历的基础上，每次判断 queque.isempty() 后，加一个queue.size
    // 并用 size 做内层循环条件，这样内层循环，只循环一层的内容
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode p = queue.poll();
            inResult.add(p.val);
            if (p.left != null) {
                queue.add(p.left);
            }
            if (p.right != null) {
                queue.add(p.right);
            }

        }

        list.add(inResult);
        inResult = new ArrayList<Integer>();
        // 错误代码， 此处不能用clear(),inResult是个指针
        // inResult.clear();
    }

    // Collections.reverse() !
    Collections.reverse(list);
    return list;

}
{% endcodeblock %}

*** 

Over！










































