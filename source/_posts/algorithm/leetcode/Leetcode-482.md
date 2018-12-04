---
title: Leetcode_482
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-11 00:17:00
---
License Key Formatting

<!-- more -->

***

# Problem
给定一个包含短横，数字，或字母的字符串，（短横只做分组用，但是这里的分组是乱的）。
现在要重新整理这个字符串。
要求是：
1. 给定一个数字 K
2. 以短横和间隔分组
3. 除了第一组以外，别的组的长度都是 K

# Solution 

``` java
public class Solution {
    public String licenseKeyFormatting(String S, int K) {
        
        StringBuilder res = new StringBuilder();
        // 计数器，数是否达到了 K
        int count = 0;
        
        int len = S.length();
        int i = len - 1;
        // 从字符串的右边向左遍历
        while(i >= 0) {
        	// 忽略字符串中原来的短横
            if(S.charAt(i) == '-'){
                i--;
                continue;
            }

            res.append(Character.toUpperCase(S.charAt(i)));
            
            count++;
            if(count == K) {
            	// 如果长度到了就加上一个短横
                res.append('-');
                // 重置计数器
                count = 0;
            }
            
            i--;
        }
        
        if(res.length()>0 && count == 0) {
        	// 如果最后添加的也是短横
        	// 那么要去掉这个短横
            res.deleteCharAt(res.length()-1);
        }
        
        return res.reverse().toString();
    }
}
```











































