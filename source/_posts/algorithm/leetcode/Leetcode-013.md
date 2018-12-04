---
title: Leetcode_013
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-10 13:08:45
---
Roman to Integer
<!-- more -->

***

### Problem
Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

看起来挺麻烦，其实也没有
就两种情况
第一种，前面数字比后面的小，那么先减后加
第二种，前面数字不比后面小，那么正常加上去就好了
 
### Solution

{% codeblock lang:java  %}
public int romanToInt(String s) {

    // 定义7个罗马数字
    int[] map = new int[256];
    map['I'] = 1;
    map['V'] = 5;
    map['X'] = 10;
    map['L'] = 50;
    map['C'] = 100;
    map['D'] = 500;
    map['M'] = 1000;

    int fast = 1;
    int slow = 0;

    char[] arr = s.toCharArray();
    int result = 0;

    while (fast < s.length()) {
        
        if (map[arr[slow]] >= map[arr[fast]]) {
            // 如果fast的数值比slow大，那就正常加上去
            result = result + map[arr[slow]];
            slow++;
            fast++;
        } else {
            // 否则，先减掉slow再加上fast
            result = result + map[arr[fast]] - map[arr[slow]];
            fast = fast + 2;
            slow = slow + 2;
        }
    }

    while (slow < s.length()) {
        result = result + map[arr[slow]];
        slow++;
    }

    return result;
}
{% endcodeblock %}

***

over！