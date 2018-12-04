---
title: Leetcode_122
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-17 21:35:00
---
Best Time to Buy and Sell Stock II


<!-- more -->

***

### Problem
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).


接Leetcode-121，这个可以做多笔交易

### Solution 
继续动态规划

重点是，一天内可以卖出一笔，同时买进一笔。

这样问题就简单很多。

{% codeblock lang:java  %}
public int maxProfit(int[] prices) {
    if (prices.length < 1) {
        return 0;
    }

    int result = 0;
    int i = 1;
    while (i < prices.length) {
        if (prices[i] > prices[i - 1]) {
            result += prices[i] - prices[i-1];
        }
        i++;
    }

    return result;
}
{% endcodeblock %}

*** 

Over！










































