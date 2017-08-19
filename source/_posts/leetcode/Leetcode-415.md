---
title: Leetcode_415
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-01 23:20:00
---
Add Strings

<!-- more -->

***

### Problem
两个字符串相加

注意点
1. 从string的最后一位开始，index要注意
2. 用Stringbuilder的话，高位都是append到后面的，所以最后要reverse一下

### Solution 

``` java
public class Solution {
    public String addStrings(String num1, String num2) {
        
        String longS = null;
        String shortS = null;
        if(num1.length() > num2.length()) {
            longS = num1;
            shortS = num2;
        } else {
            longS = num2;
            shortS = num1;
        }
         
        int longindex = longS.length() - 1;
        int shortindex = shortS.length() - 1;
        
        int i = 0;
        StringBuilder sb = new StringBuilder();
        int carry = 0;
        while(shortindex >= 0) {

        	// Solution里
        	// num1.charAt(i) - '0'这样也行………………
            int sum = (shortS.charAt(shortindex)-'0')
                + (longS.charAt(longindex)-'0')
                + carry;
            int digit = sum % 10;
            sb.append(digit);
            carry = sum / 10;
            shortindex--; 
            longindex--;
        }
        
        while(longindex>=0) {
            int sum = (longS.charAt(longindex)-'0') + carry;
            int digit = sum % 10;
            sb.append(digit);
            carry = sum / 10;
            longindex--;
        }
        
        if(carry > 0) {
            sb.append(carry);
        }
        
        return sb.reverse().toString();
    }
}
```











































