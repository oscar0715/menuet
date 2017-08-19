---
title: Leetcode_203
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-29 00:00:00
---
Remove Linked List Elements

<!-- more -->

***

### Problem
Remove all elements from a linked list of integers that have value val.

Example
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6
Return: 1 --> 2 --> 3 --> 4 --> 5



### Solution
1. 快慢指针
2. 先判断快指针
3. 这样head就会没有判断，最后再来判断head

{% codeblock lang:java  %}
public ListNode removeElements(ListNode head, int val) {

    if (head == null) {
        return head;
    }

    ListNode slow = head;
    ListNode fast = head.next;

    while(fast != null) {
        if (fast.val == val) {
            slow.next = fast.next;
            fast = slow.next;
        } else {
            slow = slow.next;
            fast = fast.next;
        }
    }

    if (head.val == val) {
        head = head.next;
    }
    return head;

}

{% endcodeblock %}


*** 

Over！










































