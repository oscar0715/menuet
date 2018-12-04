---
title: Leetcode_155
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-31 18:40:00
---

Min Stack
<!-- more -->

***

### Problem
实现一个可以获取最小值的 Stack

### Solution 二分查找

``` java
public class MinStack {

    // 一个栈存原始数据
    Stack<Integer> stack = new Stack<>();
    // 另一个栈存对应的最小值
    Stack<Integer> minStack = new Stack<>();
    
    /** initialize your data structure here. */
    public MinStack() {
    }
    
    public void push(int x) {
        stack.push(x);
        int min = Integer.MAX_VALUE;
        if(!minStack.isEmpty()) {
            min = minStack.peek();
        }
        minStack.push(Math.min(min,x));
    }
    
    public void pop() {
        stack.pop();
        minStack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```


*** 

Over！










































