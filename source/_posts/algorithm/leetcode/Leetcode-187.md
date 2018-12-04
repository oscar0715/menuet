---
title: Leetcode_187
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-02 00:33:00
---
Repeated DNA Sequences

<!-- more -->

***

### Problem
All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

For example,

    Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",

    Return:
    ["AAAAACCCCC", "CCCCCAAAAA"].


### Solution
我就遍历一遍，然后装到map里

大神貌似都是用的为运算呢，呵呵了又。

{% codeblock lang:java  %}
public List<String> findRepeatedDnaSequences(String s) {
    List<String> list = new ArrayList<String>();
    Set<String> set = new HashSet<String>();

    Map<String, Integer> map = new HashMap<String, Integer>();

    for(int i = 10; i <= s.length(); i++ ) {
        String subString = s.substring(i-10,i);
        if(map.containsKey(subString) ){
            map.put(subString, map.get(subString) + 1);
            set.add(subString);
        } else {
            map.put(subString, 1);
        }
    }
    
    for (String subString : set) {
        list.add(subString);
    }
    return list;
}
{% endcodeblock %}


*** 

Over！










































