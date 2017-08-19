---
title: Leetcode_083
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-05 14:36:00
---
Remove Duplicates from Sorted List
链表
<!-- more -->

***

### Problem
Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,
Given 1->1->2, return 1->2.
Given 1->1->2->3->3, return 1->2->3.

### Solution 有序链表
有序链表，这就很简单
{% codeblock lang:java  %}
public ListNode deleteDuplicates(ListNode head) {
        
    if (head == null ) {
        return null;
    }
    
    ListNode current = head;
    ListNode nextLink = head.next;
    
    while (nextLink != null) {
        if (current.val == nextLink.val) {
            current.next=nextLink.next;
            nextLink = nextLink.next;
        } else {
            current = current.next;
            nextLink = nextLink.next;
        }
    }
    return head;
}
{% endcodeblock %}

### Solution 无序链表
无序链表，考虑用一个hashset来存已经出现过得node

{% codeblock lang:java  %}
public ListNode deleteDuplicates(ListNode head) {

        if (head == null) {
            return null;
        }
        // default false;
        HashSet<Integer> flag = new HashSet<Integer>();

        ListNode current = head;
        ListNode nextLink = head.next;
        flag.add(head.val);

        while (current.next != null) {
            // flag.add(nextLink.val)
            if (!flag.add(nextLink.val)) {
                // 如果set中已经有了，那么add方法的结果为false
                // 该节点为重复节点
                nextLink = nextLink.next;
                current.next = nextLink;
            } else {
                current = current.next;
                nextLink = nextLink.next;
            }
        }
        return head;
    }
{% endcodeblock %}

*** 

Over！










































