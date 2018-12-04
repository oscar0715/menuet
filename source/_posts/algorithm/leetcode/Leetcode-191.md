---
title: Leetcode_191
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-15 15:14:00
---
Number of 1 Bits
<!-- more -->

***

### Problem
Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).

For example, the 32-bit integer ’11' has binary representation 00000000000000000000000000001011, so the function should return 3.

整数转为一个二进制数，然后数1的个数


### Solution1 
需要注意的是java中没有无符号数，所以负数的情况（32位的第一位为1）要特别考虑。

{% codeblock lang:java  %}
public int hammingWeight(int n) {
    int count = 0;
    if (n < 0) {
        // 当n为0时,通过减去最小的整数,转为一个正数
        n = n - Integer.MIN_VALUE;
        //把32位中第一位的1加上
        count++;
    }
    return count + countPositive(n);
}

int countPositive(int decimal) {
    int count = 0;
    while (decimal / 2 > 0) {
        if (decimal % 2 == 1) {
            count++;
        }
        decimal = decimal / 2;
    }
    if (decimal != 0) {
        count++;
    }
    return count;
}
{% endcodeblock %}

### Solution2 countPositive通过移位运算来实现
改一下 countPositive方法更快
{% codeblock lang:java  %}
int countPositive(int decimal) {
    int count = 0;
    while (decimal>0) {
        count += decimal&1;
        decimal >>= 1;
    }
    return count;
}
{% endcodeblock %}

### Solution3 还有更简单的不用对负数处理的

'<<' : 左移运算符，num << 1,相当于num乘以2

'>>' : 右移运算符，num >> 1,相当于num除以2

'>>>': 无符号右移，忽略符号位，空位都以0补齐

正数的情况反正都是一样的
当n为负数的时候

第一位为1，`>>` 右移运算的时候空位全都用1补齐，所以不能用decimal>0来判断

这个时候用无符号右移，空位都以0补齐，之后都是正数，完美规避，于是就有以下不用对负数进行处理的Solution
{% codeblock lang:java  %}
public static int hammingWeight(int n) {
    int ones = 0;
    while(n!=0) {
        ones = ones + (n & 1);
        n = n>>>1;
    }
    return ones;
}
{% endcodeblock %}

*** 

Over！










































