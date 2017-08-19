---
title: Leetcode_205
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-28 10:30:00
---
Reverse Linked List
反转单链表
<!-- more -->

***

### Problem
Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

For example,
Given "egg", "add", return true.

Given "foo", "bar", return false.

Given "paper", "title", return true.

Note:
You may assume both s and t have the same length.


就是能不能通过字母替换来实现把一个字符串转换成另外一个字符串
但是替换的字符只能一一对应，不能一对多，或者多对一


### Solution
用HashMap
{% codeblock lang:java  %}
public boolean isIsomorphic(String s, String t) {

    int length = s.length();

    Map<Character, Character> map = new HashMap<Character, Character>();

    for (int i = 0; i < length; i++) {
        if (map.containsKey(s.charAt(i))) {
            if (!map.get(s.charAt(i)).equals(t.charAt(i))) {
                return false;
            }
        } else {
            if (map.containsValue(t.charAt(i))){
                return false;
            }
        }

        map.put(s.charAt(i),t.charAt(i));
    }

    return true;
}
{% endcodeblock %}


*** 

Over！










































