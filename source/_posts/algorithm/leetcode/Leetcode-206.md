---
title: Leetcode_206
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-13 22:04:00
---
Reverse Linked List
反转单链表
<!-- more -->

***

### Problem
Reverse a singly linked list.


### Solution
当然可以用栈，但是太慢了
所以不用栈
{% codeblock lang:java  %}
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }

    ListNode pre = head;
    ListNode current = pre.next;
    // 这条不能漏,否则会变成环
    pre.next = null;

    ListNode post;
    while (current != null) {
        // post 后移
        post = current.next;

        // post后移完以后,三个指针都归为初始的顺序,正式开始一轮新的反转工作

        current.next = pre;
        // 关键在此处,current转向以后,马上赋值到pre
        pre = current;
        //current后移
        current = post;
    }

    // 每次反转完,pre都会被负值到刚刚反转结束的那个节点,所以最后一次反转完成后,应该返回pre
    return pre;
}
{% endcodeblock %}


*** 

Over！










































