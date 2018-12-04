---
title: Leetcode_225
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-07 00:22:00
---
Implement Stack using Queues

<!-- more -->

***

### Problem
Implement the following operations of a stack using queues.

    push(x) -- Push element x onto stack.
    pop() -- Removes the element on top of the stack.
    top() -- Get the top element.
    empty() -- Return whether the stack is empty.

### Solution 

1. 两个queue
2. 和232题思路简直一模一样
``` java
public class MyStack {
    
    Queue<Integer> queue;

    /** Initialize your data structure here. */
    public MyStack() {
        queue = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        Queue<Integer> queue1 = queue;
        queue = new LinkedList<>();
        queue.add(x);
        for(int i : queue1) {
            queue.add(i);
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue.poll();
    }
    
    /** Get the top element. */
    public int top() {
        return queue.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue.isEmpty();
    }
}
```



*** 

Over！






























