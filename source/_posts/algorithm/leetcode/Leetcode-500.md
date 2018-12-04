---
title: Leetcode_500
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-29 10:19:00
---
Keyboard Row

<!-- more -->

***

# Problem
找出字符串数组中，所有字母都在键盘同一行的字符串。

# Solution 


``` java
public class Solution {
    public String[] findWords(String[] words) {

        List<String> res = new ArrayList<>();
        
        
        
        Set<Character> set1 = new HashSet<>(); 
        Set<Character> set2 = new HashSet<>(); 
        Set<Character> set3 = new HashSet<>(); 
        
        String s = "qwertyuiop";
        for(int i = 0; i< s.length();i++) {
            set1.add(s.charAt(i));
        }
        s="asdfghjkl";
        for(int i = 0; i< s.length();i++) {
            set2.add(s.charAt(i));
        }
        s="zxcvbnm";
        for(int i = 0; i< s.length();i++) {
            set3.add(s.charAt(i));
        }
        
        for(String word : words) {
            String word1 = word.toLowerCase();
            if(check(word1, set1) ||
                check(word1, set2) ||
                check(word1, set3) 
            ) {
                res.add(word);        
            }
        }
        
        String [] resArray = new String[res.size()];
        resArray = res.toArray(resArray);
        
        return resArray;
    }
    
    private boolean check(String string, Set<Character> set) {
        for(int i = 0; i < string.length(); i++) {
            if(!set.contains(string.charAt(i))) {
                return false;
            }
        }
        
        return true;
    }
}
```

