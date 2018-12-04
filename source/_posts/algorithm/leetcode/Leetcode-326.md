---
title: Leetcode_326
mathjax: true
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-24 19:40:00
---
Power of Three

<!-- more -->

***

# Problem
判断数字是不是3的幂

# Solution 

时间复杂度是
$ O(\log\_3(n)) $

``` Java
    public boolean isPowerOfThree(int n) {
        
        if(n <= 0) {
            return false;
        }
        
        // 循环除
        while(n % 3 == 0) {
            
            n = n / 3;
            
        }
        
        return n == 1;
        
    }
```








































