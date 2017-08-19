---
title: Leetcode_520
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-02 22:58:00
---
Detect Capital

<!-- more -->

***

# Problem
判断一个字符串是否合理，三种合理字符串
1. “USA”
2. “leetcode”
3. “Google”

# Solution 

这是最笨的办法……

``` java
public class Solution {
    public boolean detectCapitalUse(String word) {
        if(word.length() < 2) {
            return true;
        }
        
        char first = word.charAt(0);
        if( isLower(first) ) {
            
            // first lower, all lower
            int i = 1;
            while(i < word.length()) {
                char c = word.charAt(i);
                if(!isLower(c)) {
                    return false;
                }
                i++;
            }
            
        } else {
            char second = word.charAt(1);
            
            if(isLower(second)) {
                // second lower, all lower
                int i = 2;
                while(i < word.length()) {
                    char c = word.charAt(i);
                    if(!isLower(c)) {
                        return false;
                    }
                    i++;
                }
            } else {
                // first capical and second capital
                // all capital
                int i = 2;
                while(i < word.length()) {
                    char c = word.charAt(i);
                    if(isLower(c)) {
                        return false;
                    }
                    i++;
                }
            }
        }
        
        return true;
    }
    
    private boolean isLower(char c) {
        if(c >= 'a' && c <= 'z') {
            return true;
        } 
        
        return false;
    }
}
```


