---
title: Leetcode_543
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-22 05:53:00
---
Reverse Words in a String III

<!-- more -->

***

# Problem
Example 1:
Input: "Let's take LeetCode contest"
Output: "s'teL ekat edoCteeL tsetnoc"

# Solution 

``` java
public class Solution {
    public String reverseWords(String s) {
        String[] arr = s.split("\\s+");
        StringBuilder sb = new StringBuilder();
        
        for(String str : arr) {
            for(int i = str.length()-1; i>=0; i--) {
                sb.append(str.charAt(i));
            }
            sb.append(" ");
        }
        sb.deleteCharAt(sb.length()-1);
        
        return sb.toString();
    }
}
```


