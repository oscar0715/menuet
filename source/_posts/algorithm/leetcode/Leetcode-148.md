---
title: Leetcode_148
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-23 23:39:00
---
Sort List

链表的归并排序
<!-- more -->

***

### Problem
Sort a linked list in O(n log n) time using constant space complexity.



### Solution 
归并排序
参考 UCB 61B：
**<a href="/ucb61b/lecture29-SortingI/">Sorting I</a>**
{% codeblock lang:java  %}
public ListNode sortList(ListNode head) {
    if (head == null || head .next == null) {
        return head;
    }

    ListNode middle = getMiddle(head);
    ListNode secondHalf = middle.next;
    middle.next = null;

    return mergeTwoSortedList(sortList(head), sortList(secondHalf));
}

public ListNode getMiddle(ListNode head) {
    if (head == null || head .next == null) {
        return head;
    }

    ListNode slow = head;
    ListNode fast = head;

    while(fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;
}

public ListNode mergeTwoSortedList(ListNode first, ListNode second) {
    if (first == null) {
        return second;
    }
    if (second == null) {
        return first;
    }

    ListNode head;
    ListNode node;
    if (first.val < second.val) {
        node = first;
    } else {
        node = second;
    }
    head = node;

    while (first != null && second!=null) {
        if (first.val < second.val) {
            node.next = first;
            first = first.next;
        } else {
            node.next = second;
            second = second.next;
        }
    }
    if (first == null) {
        first = second;
    }

    while(first != null) {
        node.next = first;
        first = first.next;
    }

    return head;

}
{% endcodeblock %}

*** 

Over！










































