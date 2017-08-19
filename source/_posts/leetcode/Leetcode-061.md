---
title: Leetcode_061
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-18 01:40:00
---
Rotate List

<!-- more -->

***

### Problem

Given a list, rotate the list to the right by k places, where k is non-negative.

For example:
Given `1->2->3->4->5->NULL` and k = 2,
return `4->5->1->2->3->NULL`.


### Solution
代码如下
{% codeblock lang:java %}
public ListNode rotateRight(ListNode head, int k) {
	if(k == 0 || head == null) {
		return head;
	} 
	int length = 1;
	
	ListNode node = head;
	// get the length of the list
	while(node.next != null) {
		length++;
		node = node.next;
	}
	
	// get the offset
	// k may be larger than length. then % it
	int offset = length - k%length -1;
	
	// make the list a cycle
	node.next = head;
	// node point to head
	node = head;
	
	// move 
	while(offset > 0) {
		node = node.next;
		offset--;
	}
	
	head = node.next;
	
	// beak the cycle
	node.next = null;
	return head;
}
{% endcodeblock %}

*** 

Over！










































