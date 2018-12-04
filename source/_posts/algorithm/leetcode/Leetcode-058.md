---
title: Leetcode_058
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-04 16:54:00
---
Length of Last Word
<!-- more -->

***

### Problem
Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.  这句话干什么用的太扯了。
只有一个单词的时候就返回这个单词的长度。也不是返回0

Note: A word is defined as a character sequence consists of non-space characters only.

For example, 
Given s = "Hello World",
return 5.

### Solution
s要先trim一下

从后往前数快一些。

代码如下
{% codeblock lang:java  %}
public int lengthOfLastWord(String s) {
    s = s.trim();
    char[] array = s.toCharArray();
    int i = s.length()-1;
    while (i >= 0) {
        if (array[i] == ' ') {
            return s.length()-i-1;
        }
        i--;
    }
    return s.length()-i-1;
}
{% endcodeblock %}


*** 

Over！










































