---
title: Leetcode_237
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-15 16:53:00
---
Delete Node in a Linked List

<!-- more -->

***

### Problem
Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.

一个单链表，给出要删除的节点，然后把自己删了


### Solution
单链表删除节点都要知道前一个节点，然后把，前一个接到下一个，中间这个就删掉了。

我觉得这题好无聊……

{% codeblock lang:java  %}
public void deleteNode(ListNode node) {
    //本质上仍然是删除了node的下一个
    //只不过删除之前,把下一个node的value赋到了当前的node
    node.val = node.next.val;
    node.next = node.next.next;
}
{% endcodeblock %}


*** 

Over！










































