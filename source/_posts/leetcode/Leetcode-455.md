---
title: Leetcode_455
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-03 12:17:00
---
Assign Cookies
<!-- more -->

***

### Problem
题目太长了

### Solution 

第一种思路是遍历孩子的那个数组

``` java
public class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        
        int count = 0;
        int j = 0;
        for(int i = 0; i < g.length; i++) {
            
            // 找到下一块，能匹配这个孩子的饼干
            while(j < s.length && g[i] > s[j]){
                j++;
            }    
            
            if( j < s.length) {
                count++;
                j++;
            } else {
                break;
            }
            
        }
        
        return count;
    }
}
```

第二种思路是遍历饼干数组
``` java
public class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        
        int i = 0;
        for(int j = 0; i < g.length && j < s.length; j++) {
            // 以一块饼干，去匹配孩子
            // 如果当前这个孩子可以匹配，那下一轮可以匹配下一个孩子（i++）
            // 不匹配的话，到下一轮循环去看，下一块饼干去匹配
            if(g[i] <= s[j]){
                i++;
            }    
            
        }
        
        return i;
    }
}
```









































