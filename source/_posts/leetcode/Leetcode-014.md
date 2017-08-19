---
title: Leetcode_014
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-29 11:08:45
---
Longest Common Prefix
<!-- more -->

***

### Problem
Write a function to find the longest common prefix string amongst an array of strings.
 
### Solution
要点：
1. 找到最短的字符串
2. 一个一个字符判断

{% codeblock lang:java  %}
public String longestCommonPrefix(String[] longestCommonPrefix) {
    if (longestCommonPrefix.length < 1) {
        return "";
    }

    int k = Integer.MAX_VALUE;

    // k 取 字符串数组中，最短字符串的长度
    for (int i = 0; i < longestCommonPrefix.length; i++) {
        k = Math.min(k, longestCommonPrefix[i].length());
    }

    if (k == 0) {
        return "";
    }

    // 循环判断每一个字符，直到达到k，或者发现不同马上return
    int p = 0;
    while (p < k) {
        char prev = longestCommonPrefix[0].charAt(p);
        for (int i = 0; i < longestCommonPrefix.length; i++) {
            if (prev != longestCommonPrefix[i].charAt(p)) {
                return longestCommonPrefix[0].substring(0, p);
            }
        }
        p++;
    }

    return longestCommonPrefix[0].substring(0, p);
}
{% endcodeblock %}
