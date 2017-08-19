---
title: Leetcode_151
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-12 19:14:00
---
Reverse Words in a String
<!-- more -->

***

# Problem
以单词为单位 reverse 一个 字符串


# Solution0

``` java
public class Solution {
    public String reverseWords(String s) {
        
        StringBuilder sb = new StringBuilder();
        
        int index = s.length() - 1;
        // 从最后开始遍历
        while(index >= 0) {

            // 跳过当前所有空格
            while(index >= 0 && s.charAt(index) == ' ') {
                index--;
            }
                
            if(index < 0) {
                break;
            }
            // 找到单词的结尾
            int end = index;
            
            // 找单词的开头
            while(index >= 0 && s.charAt(index) != ' ') {
                index--;
            }
            // 退出循环的时候要么已经是空格了
            // 要么index小于零
            // 所以要加1，回到单词的开头
            int start = index + 1;

            // 从 start append 到 end
            while(start <= end) {
                sb.append(s.charAt(start));
                start++;
            }
            sb.append(' ');
        }
        
        if(sb.length()>0){
            // 去掉最后的那个空格
            sb.deleteCharAt(sb.length()-1);
        }
        
        return sb.toString();
    }
}
```

### Solution2 split()函数

split("//s+");

``` java
public class Solution {
    public String reverseWords(String s) {
        String[] strs = s.trim().split("\\s+");
        
        StringBuilder sb = new StringBuilder();
        for(int i = strs.length-1; i >= 0; i--) {
            sb.append(strs[i]);
            sb.append(" ");
        }
            
        if(sb.length()>0){
            sb.deleteCharAt(sb.length()-1);
        }
        
        return sb.toString();
    }
}
```


*** 

Over！










































