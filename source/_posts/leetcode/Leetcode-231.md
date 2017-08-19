---
title: Leetcode_231, 236, 342
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-15 16:53:00
---
Power of Two, Three, four

<!-- more -->

***

### Problem
Given an integer, write a function to determine if it is a power of two.
判断是否是2，3，4的幂


### Solution Power of Two

{% codeblock lang:java  %}
public boolean isPowerOfTwo(int n) {

    if ( n > 0  && (n & (n - 1)) == 0) {
        return true;
    }
    
    return false;
}
{% endcodeblock %}

### Solution Power of Three

{% codeblock lang:java  %}
public boolean isPowerOfTwo(int n) {

    if ( n > 0  && (n & (n - 1)) == 0) {
        return true;
    }
    
    return false;
}
{% endcodeblock %}

### Solution Power of Four

{% codeblock lang:java  %}
public boolean isPowerOfFour(int n) {
    if ( n > 0  && (n & (n - 1)) == 0) {
        // 先判断是不是2的幂
        if ((n & 0x55555555) == n) {
            // 再判断 
            // 与 (0x55555555) <==> 1010101010101010101010101010101
            // 如果得到的数还是其本身，则可以肯定其为4的次方数：
            return true;
        }
    }

    return false;
}
{% endcodeblock %}

{% codeblock lang:java  %}
public boolean isPowerOfFour(int n) {
    if ( n > 0  && (n & (n - 1)) == 0) {
        // 先判断是不是2的幂
        if ((n - 1) % 3 == 0) {
            // 在确定其是2的次方数了之后
            // 只要是4的次方数，减1之后可以被3整除 
            return true;
        }
    }

    return false;
}
{% endcodeblock %}

*** 

参考 
{% link Power of Four 判断4的次方数
 http://www.cnblogs.com/grandyang/p/5403783.html Power of Four 判断4的次方数
 %}
Over！










































