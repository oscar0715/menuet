---
title: Leetcode_242
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-21 14:07:00
---
Valid Anagram

<!-- more -->

***

### Problem
Given two strings s and t, write a function to determine if t is an anagram of s.

For example,
s = "anagram", t = "nagaram", return true.
s = "rat", t = "car", return false.

Note:
You may assume the string contains only lowercase alphabets.

验证两个字符串通过调整字幕顺序以后能不能相同

### Solution 1
利用map 肯定很慢

{% codeblock lang:java  %}
public boolean isAnagram(String s, String t) {

    if (s.length() != t.length()) {
        return false;
    }

    int length = s.length();


    Map<Character, Integer> mapS = new HashMap<Character, Integer>();
    Map<Character, Integer> mapT = new HashMap<Character, Integer>();

    for (int i = 0; i < length; i++) {

        Integer count = mapS.get(s.charAt(i));
        if (count == null) {
            mapS.put(s.charAt(i), 1);
        } else {
            mapS.put(s.charAt(i), ++count);
        }

        count = mapT.get(t.charAt(i));
        if (count == null) {

            mapT.put(t.charAt(i), 1);
        } else {
            mapT.put(t.charAt(i), ++count);
        }
    }

    if (mapS.keySet().equals(mapT.keySet())) {
        for (Character key : mapS.keySet()) {
            if (!mapS.get(key).equals(mapT.get(key))) {
                return false;
            }
        }
        return true;
    } else {
        return false;
    }
}
{% endcodeblock %}

### Solution 2
两个charArray 排序后逐个比较也不快

{% codeblock lang:java  %}
public boolean isAnagram(String s, String t) {

    if (s.length() != t.length()) {
        return false;
    }

    char[] arrS = s.toCharArray();
    char[] arrT = t.toCharArray();

    Arrays.sort(arrS);
    Arrays.sort(arrT);

    int i = 0;
    while (i < s.length()) {
        if (arrS[i] != arrT[i]){
            return false;
        }
        i++;
    }
    return true;
}
{% endcodeblock %}

### Solution 3
用数组代替map

charAt 也很慢，还是先转成数组，数组定位还是最快的

{% codeblock lang:java  %}
public boolean isAnagram(String s, String t) {

    if (s.length() != t.length()) {
        return false;
    }

    int[] count = new int[26];

    for (char c : s.toCharArray()) {
        count[c - 'a']++;
    }

    for (char c : t.toCharArray()) {
        count[c - 'a']--;
    }

    for (int i = 0; i < 26; i++) {
        if (count[i] != 0) {
            return false;
        }
    }

    return true;
}
{% endcodeblock %}


*** 

Over！










































