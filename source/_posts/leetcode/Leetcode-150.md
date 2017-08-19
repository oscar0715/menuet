---
title: Leetcode_150
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-31 19:31:00
---
Evaluate Reverse Polish Notation
实现一个堆，可以实现
<!-- more -->

***

### Problem
Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Some examples:
    ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
    ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6


### Solution

用 Stack
注意除和减的时候的pop的顺序
{% codeblock lang:java  %}
public int evalRPN(String[] tokens) {
    Stack<Integer> stack = new Stack<Integer>();
    
    for(String s : tokens) {
        
        if (s.equals("+")) {
            stack.push(stack.pop() + stack.pop());
            
        } else if (s.equals("-")) {
            int b = stack.pop() ;
            int a = stack.pop();
            stack.push(a - b);
            
        } else if (s.equals("*")) {
            stack.push(stack.pop() * stack.pop());
            
        } else if (s.equals("/")) {
            
            int b = stack.pop();
            int a = stack.pop();
            stack.push(a / b);
            
        } else {
            stack.push(Integer.valueOf(s));
        }
    }
    return stack.pop();
    
}
{% endcodeblock %}



*** 

Over！










































