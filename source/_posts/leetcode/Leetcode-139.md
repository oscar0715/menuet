title: Leetcode_139
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-23 13:42:00
---
Word Break
<!-- more -->

***

### Problem
给定：
1. 字符串 s
2. 字符串列表 List<String> wordDict

判断 s 能否被拆成 wordDict 中的字符串的组合

例子

For example, given
s = "leetcode",
dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".


### Solution 

典型的动态规划

``` java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
    	// 长度设置为 s.length() + 1
    	// 我觉得主要因为 substring(j,i) 是不包括i的
        boolean[] res = new boolean[s.length() + 1];
        // res[0] 要设置为true
        res[0] = true;
        
        // for 循环的判断条件是 <=
        for(int i = 0; i <= s.length(); i++) {
            for(int j = 0; j < i; j++) {
                // 状态转移方程
                if(res[j] && wordDict.contains(s.substring(j,i))) {
                    res[i] = true;
                    break;
                }
            }
        }
        return res[s.length()];
    }
}
```

*** 

Over！










































