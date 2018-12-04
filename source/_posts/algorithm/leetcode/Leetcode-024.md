---
title: Leetcode_024
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-29 19:10:45
---
Swap Nodes in Pairs
链表
<!-- more -->

***

### Problem
Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given `1->2->3->4`, you should return the list as `2->1->4->3`.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

### Solution

``` java
public class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        
        ListNode start = new ListNode(0);
        start.next = head;

        // 三个要用的指针
        // cur 和 post 是两个要交换的节点
        ListNode pre = start;
        ListNode cur = head;
        ListNode post = head.next;
        
        // 要返回的头指针指向 post
        // 因为 post 之后交换了以后就是头指针了。
        head = post;
        
        while(true) {
            // 交换
            pre.next = post;
            cur.next = post.next;
            post.next = cur;
            
            if(cur.next == null || cur.next.next==null ) {
                break;
            }

            // 向前走
            pre = cur;
            cur = pre.next;
            post = cur.next;
        }
        
        return head;
    }
}
```
