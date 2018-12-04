---
title: Leetcode_232
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-01 11:29:00
---
Implement Queue using Stacks

<!-- more -->

***

### Problem
Implement the following operations of a queue using stacks.

    push(x) -- Push element x to the back of queue.
    pop() -- Removes the element from in front of queue.
    peek() -- Get the front element.
    empty() -- Return whether the queue is empty.

### Solution 

``` java
public class MyQueue {
    
    Stack<Integer> stack;
    
    /** Initialize your data structure here. */
    public MyQueue() {
        stack = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        Stack<Integer> stack1 = stack;
        stack = new Stack<>();
        
        stack.add(x);
        
        for(int i : stack1) {
            stack.add(i);
        }
        
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        return stack.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        return stack.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack.isEmpty();
    }
}
```

*** 

Over！










































