---
title: Leetcode_125
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-11 22:52:00
---
Valid Palindrome

回文，只留下字母数字
<!-- more -->

***

### Problem
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
`"A man, a plan, a canal: Panama"` is a palindrome.
`"race a car"` is not a palindrome.

Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.


### Solution 
时间复杂度O(n)

1. 取Alphanumeric, 只判断字母和数字
2. 大小写不要紧 用一个abs函数

{% codeblock lang:java  %}
public boolean isPalindrome(String s) {
    if (s.length() < 1) {
        return true;
    }
    char[] arr = s.toCharArray();
    int head = 0;
    int end = arr.length - 1;
    while (head < end) {
        while (!checkAlphanumeric(arr[head]) && head < end) {

            head++;
        }
        while (!checkAlphanumeric(arr[end]) && head < end) {
            end--;
        }
        if (!checkSame(arr[end], arr[head])) {
            return false;
        }

        head++;
        end--;

    }
    return true;
}

Boolean checkAlphanumeric(char c) {
    if ((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9')) {
        return true;
    }
    return false;
}

Boolean checkSame(char c1, char c2) {
    if ((c1 >= '0' && c1 <= '9') || (c2 >= '0' && c2 <= '9')){
        return c1 == c2;
    } else {
        return c1 == c2 || Math.abs(c1 - c2) == 'a' - 'A';
    }
}
{% endcodeblock %}

*** 

Over！










































