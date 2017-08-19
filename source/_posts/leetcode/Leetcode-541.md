---
title: Leetcode_541
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-02 20:36:00
---
Reverse String II

<!-- more -->

***

# Problem
给定字符串和一个整数k
每2k个长度，reverse 前 k 个字符，然后后k 个保持不变
如果最后剩下了 小于 k 个长度，那么就全部 reverse

# Solution 
这一种思路就是直接拿 stringbuilder append

``` java
public class Solution {
    public String reverseStr(String s, int k) {
        int len = s.length();
        int times = len / (2*k);
        
        StringBuilder sb = new StringBuilder();
        
        int j = 1;
        while(j <= times) {
            int index = k + (j-1) * 2 * k - 1;
            int i = k;
            // reverse part
            // length = k
            while(i > 0) {
                sb.append(s.charAt(index));
                i--;
                index--;
            }
            
            // not reverse part
            // length == k
            i = k;
            index = k + (j-1) * 2 * k;
            while(i > 0) {
                sb.append(s.charAt(index));
                i--;
                index++;
            }
            j++;
        }
        
        // reverse 
        int index = Math.min(times * 2 * k + k - 1, s.length() - 1);
        while(index >= (times * 2 * k ) ) {
            sb.append(s.charAt(index));
            index--;
        }
        
        
        // append what is left
        index = Math.min(times * 2 * k + k, s.length());
        while(index < s.length()){
            sb.append(s.charAt(index));
            index++;
        }
        
        
        
       return sb.toString();
        
    }
}
```


