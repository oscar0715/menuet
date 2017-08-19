---
title: Leetcode_020
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-29 14:48:45
---
Valid Parentheses
栈
<!-- more -->

***

### Problem
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

### Solution: Switch

要点：
1. 用栈的思想 stack

{% codeblock lang:java  %}
public boolean isValid(String s) {
    Stack<Integer> stack = new Stack<Integer>();
    for (int i = 0; i < s.length(); i++) {
        System.out.println(s.charAt(i));
        switch (s.charAt(i)) {
        case '{':
            stack.push(1);
            break;
        case '(':
            stack.push(2);
            break;
        case '[':
            stack.push(3);
            break;
        case '}':
            if (stack.isEmpty() || (!stack.pop().equals(1))) {
                return false;
            }
            break;
        case ')':
            if (stack.isEmpty() || !stack.pop().equals(2)) {
                return false;
            }
            break;
        case ']':
            if (stack.isEmpty() || !stack.pop().equals(3)) {
                return false;
            }
        }
    }
    if (stack.isEmpty() == false) {
        return false;
    }
    return true;
}
{% endcodeblock %}

### Solution: if
其实两种写法都不太优雅
反正思想和效果达到就好了
{% codeblock  lang:Java %}
public boolean isValid(String s) {
    
    if (s.length() == 0) {
        return true;
    }
    char[] arr = s.toCharArray();
    
    Stack<Character> stack = new Stack<Character>();
    stack.push(arr[0]);
    
    for(int i = 1 ; i < s.length() ; i++){
        if (!stack.empty() && isMatch(stack.peek(), arr[i]) ) {
            stack.pop();
        }else {
            stack.push(arr[i]);
        }
    }
    
    if (stack.empty()) {
        return true;
    }
    
    return false;
}

public Boolean isMatch(char left, char right) {
    if ((left == '(' && right == ')') || (left == '{' && right == '}') || (left == '[' && right == ']')) {
        return true;
    } 
    return false;
}
{% endcodeblock %}
