---
title: Leetcode_007
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-24 16:43:45
---
Reverse Integer
<!-- more -->

***

### Problem
Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321

**Have you thought about this?**
Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!

If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.

Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?

For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

### Solution 
这个题目吧，主要考的是特殊情况

1. 超过了int的范围怎么办，比如1000000003reverse的结果就是3000000001。
   此题中要求超出范围返回0
2. 最后一位是0怎么办

{% codeblock lang:java  %}
public int reverse(int x) {
    int flag = 1;
    // 此处是精髓啊！类型先设置为long，这样就不怕overflow了
    long result = 0;
    if (x < 0) {
        flag = -1;
        x = -x;
    }

    while (x > 0) {
        result = result * 10 + x % 10;
        x = x / 10;
    }
    
    result = result*flag;
    // 如果overflow再判断嘛
    if (result>Integer.MAX_VALUE||result<Integer.MIN_VALUE) {
        return 0 ;
    }

    return (int)result;
}
{% endcodeblock %}
