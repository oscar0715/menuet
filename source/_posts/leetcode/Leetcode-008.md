---
title: Leetcode_008
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-27 16:43:45
---
String to Integer (atoi)
atoi (表示ascii to integer)是把字符串转换成整型数的一个函数
<!-- more -->

***

### Problem
Implement atoi to convert a string to an integer.

Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.
（难点就是考虑所有的情况，但是……卧槽，总要有点需求吧，不然不是莫名其妙吗，呵呵）

Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front. （你倒是说的大义凌然，我最后还不是一点一点去测试你的测试器，呵呵again）

**Requirements for atoi:**
1. The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. 
2. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.


### Solution 
要求是这样的：
1. 去掉字符串开头的所有空格
2. 允许去掉空格后第一个字符是加减号，分两种情况
    - 第一有符号后面马上连数字
    - 第二无符号马上连数字
3. 数字开始后就不能有别的字符，否则就丢掉
4. 字符和数字转化 char-'0'
5. overflows以后返回INT_MAX or INT_MIN

{% codeblock lang:java  %}
public int myAtoi(String str) {

        if (str.length() == 0) {
            return 0;
        }
        str = str.trim();

        char[] strArray = str.toCharArray();

        int flag = 1;
        int i = 0;
        if (strArray[0] == '-') {
            flag = -1;
            i++;
        } else if (strArray[0] == '+') {
            i++;
        }

        int result = 0;

        for (; i < strArray.length; i++) {
            int digit = strArray[i] - '0';
            if (digit > 9 || digit < 0) {
                break;
            } else {
          
                if (digit > Integer.MAX_VALUE - result * 10 || result > Integer.MAX_VALUE / 10) {
                    return flag == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
                }
                result = result * 10 + digit;

            }
        }
        return result * flag;
    }
{% endcodeblock %}


唉 来研究一下判断条件
{% codeblock lang:java  %}
// 正确的是
if (digit > Integer.MAX_VALUE - result * 10 || result > Integer.MAX_VALUE / 10) {}

// 错误一:
if (result * 10 + digit > Integer.MAX_VALUE) {}
// 当string="2147483648",跑到最后一位时，if条件应当成立
// 214748364*10+8 = -2147483647 < Integer.MAX_VALUE 判断有误


// 错误二:
if (digit > Integer.MAX_VALUE - result * 10) {}
// 当string="10522545459",跑到倒数第二位时
// [digit] = 5 [Integer.MAX_VALUE-result*10] = 1095229107
// 条件不成立
// 那么  result = 1095229107*10+5 = 10,952,291,075 > 2,147,483,647 overflow了！

// 所以,只有
if (digit > Integer.MAX_VALUE - result * 10 || result > Integer.MAX_VALUE / 10) {}

// 当string="2147483648",跑到最后一位时，if条件应当成立
// digit > Integer.MAX_VALUE - result * 10 ==> 8>7成立

// 当string="10522545459",跑到倒数第二位时
// [result] = 1,052,254,545 [Integer.MAX_VALUE / 10] = 214,748,364
// 也就是 result * 10就比 MAX_VALUE大了
{% endcodeblock %}

这个判断乱死了
先放在这里，下次再想。