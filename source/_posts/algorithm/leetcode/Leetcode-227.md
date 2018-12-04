---
title: Leetcode_227
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-06 19:24:00
---
Basic Calculator II
<!-- more -->

***

# Problem
实现一个简单的计算器
可能有空格

# Solution 

``` java
public class Solution {
    public int calculate(String s) {
        if( s == null || s.length() == 0) {
            return 0;
        }
        
        int len = s.length();
        
        Stack<Integer> stack = new Stack<Integer>();

        // 记录当前的数字
        int num = 0;

        // 记录当前数字之前的那个符号
        char sign = '+';
        
        for( int i = 0; i < len; i++ ) {
            char c = s.charAt(i);

            // 不断取数字
            if(Character.isDigit(c)) {
                num = num*10 + (c-'0');
            }
            
            // 碰到不是数字了
            // 意味着当前的数字已经取完了
            // 就可以完成上一个符号的运算了

            // 注意这里的这个 i == len-1 非常重要
            // 这个条件负责把最后一个数字入栈
            if( !Character.isDigit(c) && c != ' ' || i == len-1){
                // 加减只要加上符号
                // 当然其实要pop前一个数字也可以，不过没有必要
                if(sign == '-') {
                    stack.push(-num);
                }
                if(sign == '+') {
                    stack.push(num);
                }
                // 乘除要pop前一个数字，计算完了以后，把结果入栈
                if(sign == '/') {
                    stack.push(stack.pop() / num);
                }
                if(sign == '*') {
                    stack.push(stack.pop() * num);
                }
                
                // 重置
                sign = c;
                num = 0;
            }
        }

        // 把栈里的结果全部加起来。
        int res = 0;
        for(int i : stack) {
            res += i;
        }
        
        return res;
    }
}
```



*** 

Over！






















































