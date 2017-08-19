---
title: Leetcode_027
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-04 08:39:00
---
Implement strStr().
<!-- more -->

***

### Problem
Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

就是在一个字符串里面，找子串的意思

### Solution
一般来说暴力破解嘛。附件复杂度为O(m·n)
这个就不说了

一种更高级方法是KMP Pattern Search
给个视频链接：{% link Knuth–Morris–Pratt(KMP) https://www.youtube.com/watch?v=GTJr8OvyEVQ Knuth–Morris–Pratt(KMP) %}
(视频里的小哥，用手指擦白板的声音，呃呃呃，慎入)
但是这个好复杂啊

还有另外一种算法 Boyer-Moore
给一个解释：{% link 字符串匹配的Boyer-Moore算法 http://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html 字符串匹配的Boyer-Moore算法 %}
代码如下
{% codeblock lang:java  %}
public int strStr(String haystack, String needle) {
    int hlen = haystack.length();
    int nlen = needle.length();

    int[] jump = new int[256]; // hashmap char-> index, assume ASCII

    // 先赋为-1
    for (int i = 0; i < jump.length; i++) {
        jump[i] = -1;
    }

    // 赋每个字母在needle中最后一次出现的位置
    for (int i = 0; i < nlen; i++) {
        jump[needle.charAt(i)] = i; // index of last occurrence
    }

    int skip = 0;
    for (int i = 0; i < hlen - nlen + 1; i += skip) { // !!! not i<hlen

        skip = 0;
        for (int j = nlen - 1; j >= 0; j--) {
            // i+j是因为要从needle的最后一个字母开始比较
            if (haystack.charAt(i + j) != needle.charAt(j)) {
                // 如果不相等，那么跳过的位数，
                skip = Math.max(1, j - jump[haystack.charAt(i + j)]);
                break;
            }
            // 如果判断的结果是相等的，那么接着往前比较j-1那个字母
        }
        if (skip == 0)
            return i;
    }
    return -1;
}
{% endcodeblock %}


*** 

Over！










































