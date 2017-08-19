---
title: Leetcode_234
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-25 17:38:00
---
Palindrome Linked List

<!-- more -->

***

# Problem

Given a singly linked list, determine if it is a palindrome.
判断链表是不是回文

# Solution1 Stack

1. stack
2. 快慢指针

``` java
public boolean isPalindrome(ListNode head) {
	if(head == null || head.next == null) {
		return true;
	}
	
	Stack<Integer> stack = new Stack<>();
	
	ListNode slow = head;
	ListNode fast = head;
	
	while(fast != null && fast.next != null) {
		stack.push(slow.val);
		slow = slow.next;
		fast = fast.next.next;
	}
	
	// 如果链表节点个数为奇数
	// 此时的fast会停在最后一个节点上
	// slow 停在中间，让slow向前进一步
	// stack装了除中间节点外，前一半的val
	if(fast != null) {
		slow = slow.next;
	}

	// 否则就是fast停在最后一个节点的next，也就是null
	// slow在厚一半的第一个节点上，
	// stack装了前一半的val
	
	while(slow!=null) {
		if(slow.val != stack.pop()) {
			return false;
		}
		slow = slow.next;
	}
	return true;
}
```

# Solution2 ReverseList

``` java
public class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null || head.next == null) {
            return true;
        }
        
        ListNode fast = head;
        ListNode slow = head;
        
        // 找到中间节点 slow
        while(fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        
        // reverse 完了以后，后半段的头儿是 end
		ListNode end = reverseList(slow);
        while(head!=null && end != null) {
            if(head.val != end.val) {
                return false;
            }
            
            head = head.next;
            end = end.next;
        }

        return true;
    }
    
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        
        ListNode pre = head;
        ListNode current = head.next;
        pre.next = null;
        
        ListNode post;
        while(current != null) {
            post = current.next;
            
            current.next = pre;
            pre = current;
            current = post;
        }
        
        return pre;
    }
}
```
*** 

Over！










































