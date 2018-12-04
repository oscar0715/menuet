---
title: Leetcode_160
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-20 10:39:00
---
Intersection of Two Linked Lists
找两个单链表的相交处
<!-- more -->

***

### Problem
Write a program to find the node at which the intersection of two singly linked lists begins.


For example, the following two linked lists:
<pre>
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
</pre>
begin to intersect at node c1.


Notes:

If the two linked lists have no intersection at all, return null.
The linked lists must retain their original structure after the function returns.
You may assume there are no cycles anywhere in the entire linked structure.
Your code should preferably run in O(n) time and use only O(1) memory.


### Solution 
1. 求两个链表的长度，然后求出长度的差值。
2. 长的链表先走，走完差值的长度，剩下的长度与短链表长度相等
3. 然后两个链表同时走，同时比较，找到相同的 node 就返回

``` java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {

    ListNode nodeA = headA;
    ListNode nodeB = headB;

    int lengthA = 0;
    int lengthB = 0;
    int diff = 0;

    // 求A长度
    while (nodeA != null) {
        lengthA++;
        nodeA = nodeA.next;
    }

    // 求B长度
    while (nodeB != null) {
        lengthB++;
        nodeB = nodeB.next;
    }

    //判断长短
    ListNode longer;
    ListNode shorter;
    if (lengthA >= lengthB) {
        diff = lengthA - lengthB;
        longer = headA;
        shorter = headB;
    } else {
        diff = lengthB - lengthA;
        longer = headB;
        shorter = headA;
    }

    // 长的先走，直到剩下的长度与短链表相等
    while (diff > 0) {
        longer = longer.next;
        diff--;
    }

    // 同时走，比较
    while (longer != null) {
        if (longer == shorter) {
            return longer;
        }
        longer = longer.next;
        shorter = shorter.next;
    }

    return null;

}
```

*** 

Over！










































