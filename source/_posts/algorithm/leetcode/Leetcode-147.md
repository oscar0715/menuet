---
title: Leetcode_147
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-25 13:13:00
---
Insertion Sort List
链表的插入排序

<!-- more -->

***

### Problem
Sort a linked list using insertion sort.

将数据分为两部分，有序部分与无序部分，一开始有序部分包含第1个元素，依次将无序的元素插入到有序部分，直到所有元素有序。插入排序又分为直接插入排序、二分插入排序、链表插入等，这里只讨论直接插入排序。它是稳定的排序算法。


### Solution 
之前思维定势，在一个链表里做

其实另外开一个新的链表就好了呀
时间复杂度 O(n)
空间复杂度 O(1)

{% codeblock lang:java  %}
public ListNode insertionSortList(ListNode head) {
    ListNode dummy = new ListNode(0);

    while (head != null) {
        ListNode node = dummy;

        while (node.next != null && node.next.val < head.val) {
            node = node.next;
        }

        ListNode tmp = head.next;
        head.next = node.next;
        node.next = head;
        head = tmp;

    }

    return dummy.next;
}
{% endcodeblock %}

*** 

Over！










































