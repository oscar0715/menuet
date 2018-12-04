---
title: Leetcode_071
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-04 13:21:00
---
Simplify Path
<!-- more -->

***

### Problem
简化路径


### Solution 
用了两个 stack 来解决
其实可以用 Deque

``` java
public class Solution {
    public String simplifyPath(String path) {
        String[] strs = path.split("/");
        Stack<String> stack = new Stack<>();
        
        String back = "..";
        String cur = ".";
        
        for(String str : strs) {
            // ".." 出栈
            if(str.equals(back)){
                if(!stack.isEmpty()) {
                    stack.pop();    
                }
                continue;
            }

            // "." 或者 "" 不变
            if(str.equals(cur) || str.equals("")){
                continue;
            }

            // 其他路劲则是入栈
            stack.push(str);
        }
        
        if(stack.isEmpty()){
            return "/";
        } else {
            // 反转stack
            Stack<String> stack1 = new Stack<>();
            while(!stack.isEmpty()){
                stack1.push(stack.pop());
            }
            stack = stack1;
            
            // 此时栈顶为根目录
            StringBuilder sb = new StringBuilder("/");
            while(!stack.isEmpty()){
                sb.append(stack.pop());
                sb.append("/");
            }
            sb.deleteCharAt(sb.length()-1);
            return sb.toString();
        }   
    }
}
```

*** 

Over！










































