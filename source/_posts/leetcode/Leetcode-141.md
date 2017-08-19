---
title: Leetcode_141
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-12 13:39:00
---
Linked List Cycle

判断链表是否有循环
<!-- more -->

***

### Problem
Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?


### Solution 
1. 快慢指针
2. 快指针每次走两步
3. 慢指针每次走一步
4. 只要有循环，快指针总能追上慢指针
5. 判读null的情况的时候，要特别小心

{% codeblock lang:java  %}
public boolean hasCycle(ListNode head) {
    if (head == null || head.next==null) {
        return false;
    }
    ListNode slow = head;
    ListNode fast = head;
    
    while(fast.next!=null && fast.next.next!=null ){
        fast=fast.next.next;
        slow = slow.next;
        if (fast == slow) {
            return true;
        }
        
    }
    return false;
}
{% endcodeblock %}

*** 

Over！










































