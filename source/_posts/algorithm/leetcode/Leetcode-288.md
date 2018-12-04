---
title: Leetcode_288
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-01 00:41:00
---
Unique Word Abbreviation

<!-- more -->

***

### Problem
https://leetcode.com/problems/unique-word-abbreviation/

网址在这里

不喜欢这种题目，讲的不清楚，不优雅


### Solution 一种思路
{% codeblock lang:java  %}
Map<String, Set<String>> map = new HashMap<String, Set<String>>();

public solution288UniqueWordAbbreviation(String[] dictionary) {
    for (String s : dictionary) {

        String abbr = getAbbr(s);
        //          System.out.println("abbr = " + abbr);

        if (map.containsKey(abbr)) {
            map.get(abbr).add(s);
        } else {
            Set<String> set = new HashSet<String>();
            set.add(s);
            map.put(abbr, set);
        }
    }

}

private String getAbbr(String s) {
    StringBuilder abbr = new StringBuilder();
    if (s.length() < 3) {
        abbr.append(s);
    } else {
        abbr.append(s.charAt(0));
        abbr.append(s.length() - 2);
        abbr.append(s.charAt(s.length() - 1));
    }

    return abbr.toString();
}

public boolean isUnique(String word) {
    String abbr = getAbbr(word);

    return !map.containsKey(abbr) || (map.get(abbr).size() == 1 && map.get(abbr).contains(word));
}
{% endcodeblock %}

*** 

Over！










































