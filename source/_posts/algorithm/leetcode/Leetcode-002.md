---
title: Leetcode_002
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-23 10:56:45
---
Add Two Numbers
<!-- more -->

***

### Problem
You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

### Solution 

链表问题嘛

1. 判断是否有null
2. 比较l1 l2 长度
3. 循环shorter
4. 循环longer比shorter长的一段
5. 判断最后的carry是否要进一位


{% codeblock lang:java %}

public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
   
    // situation null
    if (l1 == null) {
        return l2;
    }
    if (l2 == null) {
        return l1;
    }

    // get the list length
    int len1 = 0;
    int len2 = 0;

    ListNode head = l1;
    while (head.next != null) {
        len1++;
        head = head.next;
    }

    head = l2;
    while (head.next != null) {
        len2++;
        head = head.next;
    }

    // get the longer list and the shorter one
    ListNode longer = len1 >= len2 ? l1 : l2;
    ListNode shorter = len1 < len2 ? l1 : l2;

    ListNode result = null;
    ListNode sum = null;   // result points to the head of sum

    int val = 0;
    int carry = 0;   

    while (shorter != null) {
        val = longer.val + shorter.val + carry; // get the sum
        carry = val / 10; // get the carry value
        val = val % 10; // get the unit of the value

        if (sum == null) {
            sum = new ListNode(val);
            result = sum;
        } else {
            sum.next = new ListNode(val);
            sum = sum.next;
        }
        longer = longer.next;
        shorter = shorter.next;
    }

    while (longer != null) {
        val = longer.val + carry;
        carry = val / 10;
        val -= carry * 10;
        sum.next = new ListNode(val);
        sum = sum.next;
        longer = longer.next;
    }

    if (carry != 0) {
        sum.next = new ListNode(carry);
    }

    return result;
}
{% endcodeblock %}