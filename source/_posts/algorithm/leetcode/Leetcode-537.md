---
title: Leetcode_537
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-02 21:53:00
---
Complex Number Multiplication

<!-- more -->

***

# Problem
给出两个`a+bi` 格式的数 求乘积.
`i^2 = -1`

例子
```
Input: "1+-1i", "1+-1i"
Output: "0+-2i"
Explanation: (1 - i) * (1 - i) = 1 + i^2 - 2 * i = -2i, and you need convert it to the form of "0+-2i".
```

# Solution 

``` java
public class Solution {
    public String complexNumberMultiply(String a, String b) {
        String[] arrayA = a.split("\\+");
        String[] arrayB = b.split("\\+");
        
        // 获取第一个数中的 数字项
        int a1 = Integer.parseInt(arrayA[0]);
        // 获取第一个数中的 系数项
        int a2 = Integer.parseInt(arrayA[1].substring(0, arrayA[1].length()-1));
        
        int b1 = Integer.parseInt(arrayB[0]);
        int b2 = Integer.parseInt(arrayB[1].substring(0, arrayB[1].length()-1));
        
        // 求数字项
        int c1 = a1*b1 - a2*b2;
        // 求系数项
        int c2 = a1*b2 + b1*a2;
        
        return c1+"+" + c2 + "i";
    }
}
```


