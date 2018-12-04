---
title: Leetcode_006
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-23 16:43:45
---
ZigZag Conversion
<!-- more -->

***

### Problem
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

<pre>P   A   H   N
A P L S I I G
Y   I   R
</pre>
And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:

string convert(string text, int nRows);
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

### Solution 
找规律的智力题啊，没什么好说的。

规律理一下
1. 找到第一行的数所在的位置的间隔 （间隔为numRows * 2 - 2）
   例如，行数为3，第一行数字的下标间隔为4
   这个间隔用来计算周期，所以周期也为4
2. 在用当前字母所在位置去计算所在的周期
   周期=当前字母位置去对这个间隔数取余（flag = i % (numRows * 2 - 2)）
3. 再判断当前字母位置，在这个周期的前一半还是后一半
    - 如果是前一半，那么下一个字母应该放在下一行里
      flag < (numRows * 2 - 2) / 2
    - 如果是后一半，那么下一个字母要放在上一行里
      

这种题目，我也真的是醉了。

{% codeblock lang:java  %}
public String convert(String s, int numRows) {

    if (numRows == 1)
        return s;

    StringBuffer[] rows = new StringBuffer[numRows];

    for (int i = 0; i < numRows; i++) {
        rows[i] = new StringBuffer();
    }

    int k = 0;
    char[] chars = s.toCharArray();

    for (int i = 0; i < s.length(); i++) {
        rows[k].append(chars[i]);
        // 找到第一行的数所在的位置的间隔 （间隔为numRows * 2 - 2）
        // 在用当前字母所在位置去计算所在的周期
        int flag = i % (numRows * 2 - 2);

        // 再判断当前字母位置，在这个周期的前一半还是后一半
        if (flag < (numRows * 2 - 2) / 2) {
            // 如果是前一半，那么下一个字母应该放在下一行里
            k++;
        } else {
            // 否则放在上面一行
            k--;
        }
    }

    String newS = "";
    for (int i = 0; i < numRows; i++) {
        newS += rows[i];
    }

    // System.out.println(newS);

    return newS;
}

{% endcodeblock %}
