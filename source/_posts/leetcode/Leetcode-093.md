---
title: Leetcode_93
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-03 22:52:00
---
Restore IP Addresses
<!-- more -->

***

# Problem
列出一串纯数字的字符串能够转换成可行的ip地址的所有可能

回溯

# Solution 
``` java
public class Solution {
    List<String> res = new ArrayList<>();
    public List<String> restoreIpAddresses(String s) {
        dfs(s, "", 0, 0);
        return res;
    }
    
    // count 用来数，已经有多少个以点号为分隔的部分了
    // cur用来存当前的字符串，以点号为结尾
    // index 为当前走到字符串s的位置。
    private void dfs(String s, String cur, int index,int count) {
        
        if(index == s.length() && count == 4){
            // 去掉最后的点号
            res.add(cur.substring(0, cur.length()-1));
        }
        
        if(count > 4 || index== s.length()  ) {
            return;
        }
        
        if(s.charAt(index) == '0') {
            // 不能有leading zero
            dfs(s, cur+"0"+".", ++index,count+1);
        } else {
            // 回溯的主体
            String num = s.charAt(index) + "";
            while(index<s.length() && Integer.parseInt(num) <= 255) {
                // 小于等于 255 的数字都可以作为下一个部分
                dfs(s, cur+num+".", ++index, count+1);
                
                // 越界则退出
                if(index==s.length()){
                    break;
                }
                // 继续给 num 添加一位
                num = num+s.charAt(index);
            }
        }
    }
}
```


Over！










































