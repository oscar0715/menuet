---
title: Leetcode_021
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-29 15:10:45
---
Merge Two Sorted Lists
链表
<!-- more -->

***

### Problem
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

### Solution

要点：
1. 理解上没什么难点
2. while 判断条件要注意

{% codeblock lang:java  %}
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null) {
        return l2;
    }
    if (l2 == null) {
        return l1;
    }

    ListNode head = null;
    ListNode node = null;

    if (l1.val < l2.val) {
        head = l1;
        l1 = l1.next;
    } else {
        head = l2;
        l2 = l2.next;
    }

    node = head;

    // 此处为 l1 != null
    // 不是 l1.next != null
    // 因为要走完较短的那个链表
    while (l1 != null && l2 != null) {
        if (l1.val > l2.val) {
            node.next = l2;
            node = node.next;
            l2 = l2.next;
        } else {
            node.next = l1;
            node = node.next;
            l1 = l1.next;
        }
    }

    if (l1 == null) {
        node.next = l2;
    } else {
        node.next = l1;
    }
    return head;
}
{% endcodeblock %}
