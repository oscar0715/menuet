---
title: Leetcode_070
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-05 09:17:00
---
Climbing Stairs
<!-- more -->

***

### Problem
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?


### Solution 递归
这种题目第一反应是递归
{% codeblock lang:java  %}
public int climbStairs(int n) {

    if (n == 1) return 1;  
    if (n == 2) return 2;  
    return climbStairs(n-1) + climbStairs(n-2);  

}
{% endcodeblock %}

但是递归的问题是重复计算了很多分支，所以性能很差。
例如：
    + climbStairs(5)
        + climbStairs(4)
            + climbStairs(3)
                + climbStairs(2)
                + climbStairs(1)
            + climbStairs(2)
        + climbStairs(3)
            + climbStairs(2)
            + climbStairs(1)

climbStairs(3)计算了两次，所以有重复。

### Solution 动态规划法填表（初级版）
{% codeblock lang:java  %}
public int climbStairs(int n) {

    int[] a = new int[n+1];
    a[0] = 1;
    a[1] = 1;

    for (int i = 2; i <= n; i++) {
        a[i] = a[i-1] + a[i-2];
    }
    return a[n];
}
{% endcodeblock %}

依然用climbStairs(5)举例
    + climbStairs(5)
        + climbStairs(4)
            + climbStairs(3)
                + climbStairs(2)
                + climbStairs(1)
            + climbStairs(2)
        + climbStairs(3)

此时 climbStairs(3) 不用再次计算，直接调用就好了。

### Solution 动态规划法填表（节约空间进阶版）
{% codeblock lang:java  %}
public int climbStairs(int n) {

    int[] a = new int[3];
    a[0] = 1;
    a[1] = 1;

    for (int i = 2; i <= n; i++) {
        a[i % 3] = a[(i - 1) % 3] + a[(i - 2) % 3];
    }
    return a[n % 3];
}
{% endcodeblock %}
不需要一个长度为n+1的数组。真是高手呀！。

*** 

Over！










































