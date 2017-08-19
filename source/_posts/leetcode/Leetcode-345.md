---
title: Leetcode_345
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-27 17:20:00
---
Reverse Vowels of a String

<!-- more -->

***

### Problem
Write a function that takes a string as input and reverse only the vowels of a string.

Example 1:
Given s = "hello", return "holle".

Example 2:
Given s = "leetcode", return "leotcede".

Note:
The vowels does not include the letter "y".


### Solution 一种思路

``` java
public class Solution {
    public String reverseVowels(String s) {
        
        if(s.length()<2){
            return s;
        }
        
        char[] array = s.toCharArray();
        
        int head = 0;
        int tail = s.length() - 1;
        
        while(head < tail) {    
            
            if(!isVowel(array[head])){
                head++;
            }
            
            if(!isVowel(array[tail])){
                tail--;
            }
            
            if(isVowel(array[head]) && isVowel(array[tail]) ) {
                char tmp = array[head];
                array[head] = array[tail];
                array[tail] = tmp;
                head++;
                tail--;
            } 
        }
        
        return new String(array);
    }
    
    private boolean isVowel(char c) {
        if(c == 'a' || 
            c == 'e' || 
            c == 'i' ||
            c == 'o' ||
            c == 'u' ||
            c == 'A' ||
            c == 'E' ||
            c == 'I' ||
            c == 'O' ||
            c == 'U' ) {
                return true;
            }
        return false;
    }
}
```



*** 

Over！










































