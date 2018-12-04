---
title: Leetcode_91
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-04 12:48:00
---
Decode Ways
<!-- more -->

***

### Problem
给定一个数字字符串，解码成字母字符串

### Solution 
动态规划
``` java
public class Solution {
    public int numDecodings(String s) {
        if(s.length() == 0 ) {
            return 0;
        }
        
        int[] count = new int[s.length() + 1];
        
        count[0] = 1;

        if(s.charAt(0) != '0') {
            count[1] = 1;
        }else {
            // 开头是零的话 
            // 直接 return 0
            return 0;
        }
        
        for(int i = 2; i <= s.length(); i++) {
            int first = Integer.parseInt(s.substring(i-1,i));
            int second = Integer.parseInt(s.substring(i-2,i));
            
            if(first > 0) {
               count[i] += count[i-1];
            }
            
            if(second > 9 && second <=26) {
                count[i] += count[i-2];
            }
        }
        
        return count[s.length()];
    }
}
```


*** 

Over！










































