---
title: Leetcode_434
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-03 00:02:00
---
Number of Segments in a String

<!-- more -->

***

### Problem
数一个字符串中的单词数，用空格间隔

### Solution 


``` java
public class Solution {
    public int countSegments(String s) {
        int count = 0;
        
        // 找到头
        int head = 0;
        while(head<s.length() && s.charAt(head) == ' ') {
            head++;
        }
    
        while( head<s.length() ) {
            if(s.charAt(head) != ' '){
                count++;
            }
            // 走完这一串字
            while(head<s.length() && s.charAt(head) != ' '){
                head++;
            }
            // 走完空格
            while(head<s.length() && s.charAt(head) == ' '){
                head++;
            }
        }
        
        return count;
    }
}
```











































