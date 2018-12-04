---
title: Leetcode_009
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-27 20:59:45
---
Palindrome Number
回文数
<!-- more -->

***

### Problem
Determine whether an integer is a palindrome. Do this without extra space.
 
### Solution 容易理解版

{% codeblock lang:java  %}
public boolean isPalindrome(int n) {
    long reverse = 0;
    int val = n;
    while(n>0){
        reverse = reverse*10+n%10;
        n/=10;
    }
    
    return reverse==val;
    
}
{% endcodeblock %}




### Solution 速度更快版
相比容易理解版，这个版本，不需要跑完整的循环，一旦有错，马上return false
{% codeblock lang:java  %}
public boolean isPalindrome(int n) {
        if (n < 0) {
            return false;
        }

        int div = 1;

        while (n / div >= 10) {
            div = div * 10;
        }
        // 想要n/div<10,div的位数需要与n相同

        int first = 0;
        int last = 0;
        while (n > 0) {
            
            // 取n最后一位
            last = n % 10;
            // 取n的第一位
            first = n / div;
            if (first != last) {
                return false;
            }
            
            // n % div 去掉了首位
            // 再/10 去掉了最后一位
            n = n % div / 10;
            div = div / 100;

        }

        return true;
    }

{% endcodeblock %}
