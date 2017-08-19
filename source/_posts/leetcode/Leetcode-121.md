---
title: Leetcode_121
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-17 15:53:00
---
Best Time to Buy and Sell Stock

动态规划，给一列数组，找出最大的差值，前小后大
<!-- more -->

***

### Problem
Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.


    Example 1:
    Input: [7, 1, 5, 3, 6, 4]
    Output: 5
    max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)

    Example 2:
    Input: [7, 6, 4, 3, 1]
    Output: 0
    In this case, no transaction is done, i.e. max profit = 0.

### Solution 
时间复杂度O(n)

动态规划

用一个指针，走完整个数组
同时，用一个maxDiff来存当前的最大差价

从买入价这边来看，
跟着指针走的时候总找更小的买入价。

从卖出价这边来看，
我们只关心当前价格与买入价的差值是否比当前的maxDiff大，如果大了就换。

有时候最大差价的买入价不一定是买入价最小，但是没关系，maxDiff已经存下来了这个最大差价，所以买入价在后面被换掉也没有关系。

{% codeblock lang:java  %}
public int maxProfit(int[] prices) {
    if (prices.length < 2) {
        return 0;
    }

    int minPrice = prices[0];
    int maxDiff = 0;

    int i = 1;
    // 每次的价格都可以当做买入和卖出价两个方面来看
    while (i < prices.length) {

        if (minPrice > prices[i]) {
            // 当做买入价来看
            minPrice = prices[i];
        } else {
            // 当做卖出价来看
            if (prices[i] - minPrice > maxDiff) {
                maxDiff = prices[i] - minPrice;
            }
        }

        i++;
    }
    return maxDiff;
}
{% endcodeblock %}

*** 

Over！

参考: {% link 【121-Best Time to Buy and Sell Stock（最佳买卖股票的时间）】 http://blog.csdn.net/DERRANTCM/article/details/47651235 【121-Best Time to Buy and Sell Stock（最佳买卖股票的时间）】 %}










































