---
title: Leetcode_394
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-28 20:48:00
---
Decode String

<!-- more -->

***

### Problem
Examples:

	s = "3[a]2[bc]", return "aaabcbc".
	s = "3[a2[c]]", return "accaccacc".
	s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
	7 -> 6 -> 3 -> 2 -> 1

### Solution 

``` java
public class Solution {
    public String decodeString(String s) {
        String res = "";
        Stack<String> stack = new Stack<>();
        Stack<Integer> countStack = new Stack<>();
        
        int i = 0;
        while( i < s.length()) {
            
            if(isNum(s.charAt(i))) {
            	// 碰到数字的时候
            	// 把次数加到次数数组里
                int num = 0;
                while(isNum(s.charAt(i))) {
                    num = 10 * num + Integer.parseInt(s.charAt(i)+"");
                    i++;
                }
                countStack.push(num);
            } else if(s.charAt(i) == '[') {
            	// 碰到'[' 意味着要开始新的一层循环
            	// 先把 res 压栈
            	// 新建一个 res
                stack.push(res);
                res = "";
                i++;
            } else if(s.charAt(i) == ']') {
            	// 碰到 ']' 意味着当前循环结束了
            	// 构造当前这一层循环的 string 
            	// 弹出上一层的 string 
            	// 在 string 的基础上append 这一层的res
                StringBuilder sb = new StringBuilder(stack.pop());
                int count = countStack.pop();
                int j = 0; 
                while(j < count) {
                    sb.append(res);
                    j++;
                }
                res = sb.toString();
                // 然后形成新的这一层的res
                i++;
            } else {
                res += s.charAt(i);
                i++;
            }
        }
        return res;
    }
    
    private boolean isNum(char c) {
        if( '0' <= c && c <= '9') {
            return true;
        }
        return false;
    }
}
```











































