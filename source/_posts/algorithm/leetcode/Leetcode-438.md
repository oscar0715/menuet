---
title: Leetcode_438
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-29 20:00:00
---
Find All Anagrams in a String

<!-- more -->

***

### Problem
给定两个字符串 s 和 p，找出所有 p 在 s 中的 Anagram 的起始位置

### Solution 
解决做法叫做 slide window solution


``` java
public class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new ArrayList<Integer>();
        
        // 字符hash表
        int[] hash = new int[256];
        for(int i = 0; i < p.length(); i++) {
            char c = p.charAt(i);
            hash[c]++;
        }
        
        int left = 0;
        int right = 0;
        int count = p.length();
        while(right < s.length()) {
            if(hash[s.charAt(right++)]-- >= 1) {
                // 如果s中的当前字符，在hash表中（>=1）
                // 那么 count 可以自减
                // 注意这时候如果不在hash中的字符的数量会掉到 0 以下
                count--;
            }
            
            if(count == 0) {
                // 如果count 减完了，那么
                // 意味着找到了这个 anagram 的最右边
                // 把左边加进到 res 里
                res.add(left);
            }
            
            // 当窗口的大小大于p.lenth()的时候，就要移动left来保证窗口的大小
            // 移动的同时，要把 hash 加回去
            // 注意只递增在 hash 中的字符 (就是保持非负的那些)
            // 记得第一个 if 那里么？
            // 同时加上的时候要 count++ 
            if(right - left == p.length() && hash[s.charAt(left++)]++ >= 0) {
                count++;
            }
        }   
        
        return res;
    }
}
```











































