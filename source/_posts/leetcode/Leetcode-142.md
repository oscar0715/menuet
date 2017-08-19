---
title: Leetcode_142
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-18 00:20:00
---
Linked List Cycle II

判断链表是否有循环，并且找到这个点
<!-- more -->

***

### Problem
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.


### Solution 
// 找到快指针追到慢指针的点
1. 快慢指针
2. 快指针每次走两步
3. 慢指针每次走一步
4. 只要有循环，快指针总能追上慢指针
5. 判读null的情况的时候，要特别小心


// 接下来开始找环的起点
1. 设head到环的起点的距离为x，环起点到slow和fast交汇处的距离为y
3. 所以slow走的距离为x+y
4. 设fast追上x时，比slow多走了z步
4. 所以fast走了x+y+z
5. 同时fast走了slow的两倍
6. 所以2(x + y) = x + y +z
7. 所以 x = z - y
8. 此时把slow指向head
9. fast仍然指向交汇处
10. slow和fast同时开始走，相遇时即为环开始的点

{% codeblock lang:java  %}
public ListNode detectCycle(ListNode head) {
	if(head == null || head.next == null || head.next.next == null) {
		return null;
	}
	ListNode fast = head;
	ListNode slow = head;
	
	while (fast.next!= null && fast.next.next != null) {
		fast = fast.next.next;
		slow = slow.next;
		if( fast == slow) {
			break;
		}
	}
	if(fast != slow){
		return null;
	}
	
	slow = head;
	while (slow != fast) {
		slow = slow.next;
		fast = fast.next;
	}
	
	return slow;
}
{% endcodeblock %}

*** 

Over！










































