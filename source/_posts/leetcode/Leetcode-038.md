---
title: Leetcode_038
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-04 16:02:00
---
Count and Say
<!-- more -->

***

### Problem
The count-and-say sequence is the sequence of integers beginning as follows:
1, 11, 21, 1211, 111221, ...

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
Given an integer n, generate the nth sequence.

Note: The sequence of integers will be represented as a string.

这题目说的不是人话

人话就是这是个数列
第一个数： 1
第二个数： 11 （怎么来的，读第一个数，1个1）
第三个数： 21 （怎么来的，读第二个数，2个1）
第四个数： 1211 （怎么来的，读第三个数，1个2，1个1）

input：n 
output：第n个数

### Solution

``` java
public class Solution {
    public String countAndSay(int n) {
        
        String s = "1";
        while(n > 1) {
            s = say(s);
            n--;
        }
        
        return s;
    }
    
    private String say(String s) {
        
        
        StringBuilder sb = new StringBuilder();
        int count = 1;
        char cur = s.charAt(0);
        for(int i = 1; i < s.length(); i++){
            char c = s.charAt(i);
            if(c == cur) {
                count++;
            } else {
                sb.append(count);
                sb.append(cur);
                count = 1;
                cur = c;
            }
        }
        
        sb.append(count);
        sb.append(cur);
        return sb.toString();
        
    }
}
```



*** 

Over！










































