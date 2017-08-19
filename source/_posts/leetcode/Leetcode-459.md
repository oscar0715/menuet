---
title: Leetcode_448
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-03 12:17:00
---
Repeated Substring Pattern

<!-- more -->

***

### Problem
判断一个字符串是否能由一个重复的子字符串组成

### Solution 

没用算法的方法，就是比较慢

``` java
public class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int length = s.length() / 2;
        
        for( int i = 1; i <= length; i++){
            if(s.length() % i == 0) {
                // 每次从开头取长度为 i 的字符串
                int count = s.length() / i ;
                
                StringBuilder sb = new StringBuilder();
                String substr = s.substring(0,i);

                // 然后不断重复到长度和 s 一样
                while(count>0) {
                    sb.append(substr);
                    count--;
                }

                // 判断是否相等
                if(sb.toString().equals(s)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```











































