---
title: Cracking the Code Interview 1.6
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-10-15 16:10:00
---
Compress String 

<!-- more -->

***

# Problem
Compress String 
"aabcccccaaa"  to "a2b1c5a3"

return the shorter string bewteen original string and the compressed string

# Solution 1
Normal Solution
{% codeblock lang:java  %}
    public static String compression(String s) {
        if (s == null || s.length() == 0) {
            return s;
        }

        StringBuilder result = new StringBuilder();

        char[] chars = s.toCharArray();

        char current = chars[0];
        int count = 1;

        for (int i = 1; i < chars.length; i++) {
            if (chars[i] == current) {
                count++;
            } else {
                result.append(current);
                result.append(count);
                current = chars[i];
                count = 1;
            }
        }

        result.append(current);
        result.append(count);

        if (s.length() <= result.length()) {
            return s;
        } else {
            return result.toString();
        }
    }

{% endcodeblock %}

# Solution 2
You can count the length of the String after compresstion first, then decide whether return the compressed String or the original String.

***

Over！
