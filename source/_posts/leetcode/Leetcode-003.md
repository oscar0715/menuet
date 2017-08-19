---
title: Leetcode_003
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-23 16:43:45
---
Longest Substring Without Repeating Characters
<!-- more -->

***

### Problem
Given a string, find the length of the longest substring without repeating characters.

Examples:

Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.

Given `"bbbbb"`, the answer is `"b"`, with the length of 1.

Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a substring, `"pwke"` is a subsequence and not a substring.

### Solution 
取字符串中最长无重复子串的长度

要点：

1. 字符串转化为字符数组
2. 遍历字符数组
3. boolean[] flag = new boolean[256];
4. 考虑start的位置
5. flag True 从相同的的字符开始

{% codeblock lang:java  %}

public int lengthOfLongestSubstring(String s) {

        if (s == null)
            return 0;

        boolean[] flag = new boolean[256];

        int result = 0;
        int start = 0;  //每次数一个新的字串长度的起始位置

        char[] arr = s.toCharArray();

        for (int i = 0; i < arr.length; i++) {
            // 取当前的字母
            char current = arr[i];
            // 判断当前字母是否在当前字串中出现过
            if (flag[current]) {
                // 如果出现过，那么跳出，计算长度
                result = Math.max(result, i - start);
                
                // 找这段字串中，重复的第一个字母的位置
                for (int k = start; k < i; k++) {
                    if (arr[k] == current) {
                        // 找到以后，从这个位置的下一个位置，重新开始计算长度。
                        // 此次重复的字母仍然要被标记
                        start = k + 1;
                        break; // break 开始新的外层循环
                    }
                    // 没找到就把这个字母的flag设置为false
                    flag[arr[k]] = false;
                }
            } else {
                // 如果当前子串还没出现重复的字母，标记当前字母
                flag[current] = true;
            }
        }

        // 因为前面只有重复的时候才会计算，所以这里需要另外计算一次
        result = Math.max(arr.length - start, result);

        return result;
    }

{% endcodeblock %}

探索一下这个flag数组
{% codeblock lang:java  %}
/// 判断字符串中是否存在重复字符
/// 该算法假设的前提条件：所有字符都是ASCII
/// 时间复杂度O(n),n=s.Length
/// 空间复杂度为常数O(256)
/// 时间复杂度已经最低了，空间复杂度还能有更优化的解法嘛？
boolean[] flag = new boolean[256]
{% endcodeblock %}