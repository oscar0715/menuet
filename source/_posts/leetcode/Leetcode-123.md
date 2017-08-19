---
title: Leetcode_123
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-18 19:01:00
---
Best Time to Buy and Sell Stock III


<!-- more -->

***

### Problem
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).


接Leetcode-121，这个只能做两笔交易

### Solution 
继续动态规划


解释：
首先，因为能买2次（第一次的卖可以和第二次的买在同一时间），但第二次的买不能在第一次的卖左边。
所以维护2个表，`tmpForward`和`tmpBackward`，size都和prices一样大。
意义：
`tmpForward[i]`表示 -- 截止到i下标为止，左边所做交易能够达到最大profit；
`tmpBackward[i]`表示 -- 截止到i下标为止，右边所做交易能够达到最大profit；
那么，对于`tmpForward` + `tmpBackward`，寻求最大即可。

{% link [leetcode]Best Time to Buy and Sell Stock III http://www.cnblogs.com/lihaozy/archive/2012/12/19/2825525.html [leetcode]Best Time to Buy and Sell Stock III %}

{% codeblock lang:java  %}
public int maxProfit(int[] prices) {

    if (prices.length <= 1) {
        return 0;
    }
    int result = 0;

    int[] tmpForward = new int[prices.length];

    // tmpForward[i] 的意义为,到i点为止,i左侧可以成交的最大一笔交易
    int buyPrice = prices[0];
    tmpForward[0] = 0;
    for (int i = 1; i < prices.length; i++) {
        // 作为买入来看
        if (prices[i] < buyPrice) {
            buyPrice = prices[i];
            tmpForward[i] = tmpForward[i - 1];
        } else {
            // 作为卖出来看
            if ((prices[i] - buyPrice) > tmpForward[i - 1]) {
                tmpForward[i] = prices[i] - buyPrice;
            } else {
                tmpForward[i] = tmpForward[i - 1];
            }
        }
    }


    // 反向思考
    // 从卖出一侧开始
    // tmpBackward[i] 的意义为,到i点为止,i右侧可以成交的最大一笔交易
    int[] tmpBackward = new int[prices.length];

    int sellPrice = prices[prices.length - 1];
    tmpBackward[prices.length - 1] = 0;
    for (int i = prices.length - 2; i >= 0; i--) {
        // prices[i] 作为卖出价
        if (prices[i] > sellPrice) {
            sellPrice = prices[i];
            tmpBackward[i] = tmpBackward[i + 1];
        } else {
            // prices[i] 作为买入价来看
            if ((sellPrice - prices[i] ) > tmpBackward[i + 1]) {
                tmpBackward[i] = sellPrice - prices[i];
            } else {
                tmpBackward[i] = tmpBackward[i + 1];
            }
        }
    }
    
    for (int i = 0; i<prices.length;i++){
        if ((tmpBackward[i]+tmpForward[i])>result){
            result = tmpBackward[i]+tmpForward[i];
        }
    }

    return result;
}
{% endcodeblock %}

*** 

Over！










































