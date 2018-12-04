---
title: Leetcode_019
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-29 12:48:45
---
Remove Nth Node From End of List
到链表了！
<!-- more -->

***

### Problem
Given a linked list, remove the nth node from the end of list and return its head.

For example,

   >Given linked list: 1->2->3->4->5, and n = 2.
    After removing the second node from the end, the linked list becomes 1->2->3->5.

Note:
Given n will always be valid.
Try to do this in one pass.

### Solution
链表！

要点：
1. 快慢指针

{% codeblock lang:java  %}
public ListNode removeNthFromEnd(ListNode head, int n) {

    ListNode fast = head;
    ListNode slow = head;

    int i = 0;

    // slow不动，先走fast    
    while (i < n) {
        fast = fast.next;
        i++;
    }

    // 1 --> 2 --> 3 --> 4 --> 5
    // 当n=2
    // slow为1
    // fast为3

    // 特殊情况，fast 到null，那n就是整个数组的长度
    // 移除第一个就好了
    if (fast == null) {
        head = head.next;
        return head;
    }

    // 然后 fast 和 slow 一起走
    // fast 走到头的时候，slow 的下一个就是要移除的元素
    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }
    
    // fast为5
    // slow为3
    // slow与fast差n，指向要被删除的节点的前一个节点
    // 要被删除的节点实际与最后一个节点相差n-1

    slow.next = slow.next.next;
    return head;

}

// 测试用main函数
public static void main(String[] args) {
    Test s = new Test();
    ListNode head = new ListNode(1);
    head.next = new ListNode(2);
    head.next.next = new ListNode(3);
    head.next.next.next = new ListNode(4);
    head.next.next.next.next = new ListNode(5);
    ListNode result = s.removeNthFromEnd(head, 2);
    while (result != null) {
        System.out.print(result.val + " --> ");
        result = result.next;
    }
}
{% endcodeblock %}
