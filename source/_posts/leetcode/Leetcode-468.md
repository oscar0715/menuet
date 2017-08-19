---
title: Leetcode_468
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-03 18:56:00
---
Validate IP Address

<!-- more -->

***

### Problem
给定一个字符串
判断是 IPv4 还是 IPv6，还是 "Neither"

### Solution 
头尾不能是'.' 或者 ':'

IPv4:
1. 必须四段
2. 必须是小于等于255的正整数，或者一个0
3. 不能有 leading 0

IPv6:
1. 必须有8段
2. 长度必须小于等于4，不能为空
3. 要么是数字，要么是 'a-f'的字母，大小写都可以

``` java
public class Solution {
    public String validIPAddress(String IP) {
        
        if(IP.length() == 0 ) {
            return "Neither";
        }
        
        if(checkV4(IP)) {
            return "IPv4";
        }
        
        if(checkV6(IP)) {
            return "IPv6";
        }
        
        return "Neither";
        
    }
    
    private boolean checkV4(String ip) {
        
        if(!checkHeadAndTail(ip)){
            return false;
        }
        
        String[] strs = ip.split("\\.");
        // 必须为4段
        if(strs.length != 4){
            return false;
        }
        
        for(String s : strs) {
            // 不能有负值或者长度为0
            // 也就是连续 "1..1" 这样
            if(s.length() == 0 || s.charAt(0) == '-' ){
                return false;
            }
            
            // 不能有 leading 0
            if(s.length() > 1 && s.charAt(0) == '0' ){
                return false;
            }
            
            // 必须小于255
            try {
                if(Integer.parseInt(s) > 255 || Integer.parseInt(s)  < 0) {
                    return false;
                }
                
            } catch (NumberFormatException e) {
                return false;
            }   
            
        }
        
        return true;
    }
    
    private boolean checkV6(String ip) {
        // 统一转换小写处理
        ip = ip.toLowerCase();
        
        if(!checkHeadAndTail(ip)){
            return false;
        }
        String[] strs = ip.split(":");
        
        // 必须有8段
        if(strs.length!=8) {
            return false;
        }
        
        for(String s : strs) {
            // 长度必须小于等于4，而且不能为空
            if(s.length() == 0 || s.length()>4) {
                return false;
            }
            
            for(int i = 0; i< s.length(); i++){
                char c = s.charAt(i);
                // 必须是数字或者 a-f的字母
                if(c >= '0' && c <= '9') {
                    continue;
                }
                
                if(c>= 'a' && c <='f') {
                    continue;
                }
                
                return false;
            }
        }
        
        return true;
    }
    
    private boolean checkHeadAndTail(String s) {
        // 头尾不能是 '.' 或者 ':'
        char head = s.charAt(0);
        char tail = s.charAt(s.length()-1);
        if(head == '.' || head == ':' || tail == '.' || tail == ':') {
            return false;
        }
        return true;
    }
}
```











































