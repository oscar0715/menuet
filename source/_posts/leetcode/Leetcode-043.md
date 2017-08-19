---
title: Leetcode_043
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-04 14:11:00
---
Multiply Strings
<!-- more -->

***

### Problem
给定两个字符串，求积

### Solution

笨办法

``` java
public class Solution {
    public String multiply(String num1, String num2) {
        if(num1.equals("0") || num2.equals("0")) {
            return "0";
        }
        
        List<String> list = new ArrayList<>();
        
        // get list of the product with every digit
        for(int i = num1.length()-1, digit = 0; i>=0; i--, digit++) {
            int carry = 0;
            StringBuilder sb = new StringBuilder();
            
            // get char in num1
            char c1 = num1.charAt(i);
            int n1 = Character.getNumericValue(c1);
           
            for(int j = num2.length()-1; j>=0; j--) {
                // get char in num2
                char c2 = num2.charAt(j);
                int n2 = Character.getNumericValue(c2);
                
                // get product
                int product = n1*n2+carry;
                
                // append 
                sb.append(product%10);
                
                // update carry
                carry = product / 10;
            }
            
            if(carry != 0) {
                sb.append(carry);
            }
            
            sb = sb.reverse();
            int k = digit;
            // append 0
            while(k > 0) {
                sb.append("0");
                k--;
            }
            // add to list
            list.add(sb.toString());
        }
        
        // do sum
        String s = "0";
        for(int i = 0; i < list.size(); i++) {
            s = getSum(s, list.get(i));
        }
        
        return s;
    }
    
    private String getSum(String s1, String s2) {
        String longer;
        String shorter;
        
        if(s1.length() > s2.length()){
            longer = s1;
            shorter = s2;
        } else {
            longer = s2;
            shorter = s1;
        }
        
        int carry = 0;
        
        StringBuilder res = new StringBuilder();
        int i = shorter.length()-1;
        int j = longer.length()-1;
        for(; i >=0 ; i--, j--) {
            int n1 = Character.getNumericValue(shorter.charAt(i));
            int n2 = Character.getNumericValue(longer.charAt(j));
            int sum = carry + n1 + n2;
            res.append(sum%10);
            carry = sum / 10;
        }
        
        for(; j >= 0; j--){
            int n2 = Character.getNumericValue(longer.charAt(j));
            int sum = carry + n2;
            res.append(sum%10);
            carry = sum / 10;
        }
        
        if(carry != 0) {
            res.append(carry);
        }
        
        return res.reverse().toString();
    }
}
```


*** 

Over！










































