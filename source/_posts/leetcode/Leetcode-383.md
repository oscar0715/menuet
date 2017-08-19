---
title: Leetcode_383
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-27 17:33:00
---
Ransom Note

<!-- more -->

### Solution 
送分题
还是用一个长度为256的int数组来记录magazine中字母出现过的次数

``` java
public boolean canConstruct(String ransomNote, String magazine) {
    int[] chars = new int[256];
    for(int i = 0; i < magazine.length(); i++) {
        chars[magazine.charAt(i)]++;
    }
    
    for(int i = 0; i < ransomNote.length(); i++) {
        chars[ransomNote.charAt(i)]--;
        if(chars[ransomNote.charAt(i)] < 0) {
            return false;
        }
    }
    
    return true;
}
```







































