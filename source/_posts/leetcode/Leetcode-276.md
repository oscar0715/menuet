---
title: Leetcode_276
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-26 15:07:00
---
Paint Fence 

<!-- more -->

***

### Problem
There is a fence with n posts, each post can be painted with one of the k colors.

You have to paint all the posts such that no more than two adjacent fence posts have the same color.

Return the total number of ways you can paint the fence.

Note:
n and k are non-negative integers.

数篱笆木桩的涂色方法数量，规则是，不能出现连着三根木桩都是同色的。

### Solution1  
动态规划

用两个数组
same[i] 记录第i根木桩与前一根木桩相同的涂色方法
diff[i] 记录第i根木桩与前一根木桩不同的涂色方法

same[i] + diff[i] 就是第i根木桩总的涂色方法。

但是这种方法 空间复杂度为O(n)， 实际上用了2n

{% codeblock lang:java  %}
public int numWays(int n, int k) {
  if (n <= 0 || k <= 0) {
    return 0;
  }

  if (n == 1) {
    return k;
  }

  int[] same = new int[n];
  int[] diff = new int[n];

  same[0] = 0;
  diff[0] = k;

  int i = 1;
  while (i < n) {
    same[i] = diff[i - 1];
    diff[i] = (k - 1) * (same[i - 1] + diff[i - 1]);
    i++;
  }

  return same[n - 1] + diff[n - 1];
}
{% endcodeblock %}

### Solution2 
动态规划

之前用了2个数组
状态转移方程为
  diff[i] = (k - 1) * (same[i - 1] + diff[i - 1]);

又因为
  same[i] = diff[i - 1];
  same[i - 1] = diff[i - 2];

所以状态转移方程为
  diff[i] = (k - 1) * (diff[i - 2] + diff[i - 1]);

空间复杂度仍然为O(n)， 不过实际已经减少到n了

{% codeblock lang:java  %}
public int numWays(int n, int k) {
  if (n <= 0 || k <= 0) {
    return 0;
  }

  if (n == 1) {
    return k;
  }

  int[] dp = new int[n];


  dp[0] = k;
  // 此处dp数组还是表示不同的数量
  dp[1] = k * (k - 1);


  int i = 1;
  while (i < n) {
    dp[i] = (k - 1) * (dp[i - 2] + dp[i - 1]);
    i++;
  }

  // 这里 dp[n - 2] 其实代表着 same[i - 1]
  return dp[n - 1] + dp[n - 2];
}
{% endcodeblock %}

做一下改动
  dp[1] = k * k;

状态转移方程的理解为
dp[i] = (k - 1) * (dp[i - 2] + dp[i - 1]);

dp[i - 2] 为第i-2根柱子的涂色方法
dp[i - 1] 为第i-1根柱子的涂色方法
第i根要么与i-2根不同，要么与i-1根不同

{% codeblock lang:java  %}
public int numWays(int n, int k) {
  if (n <= 0 || k <= 0) {
    return 0;
  }

  if (n == 1) {
    return k;
  }

  int[] dp = new int[n];


  dp[0] = k;
  // 此处dp[i] 包含了 same和diff的两种情况
  dp[1] = k * k;


  int i = 1;
  while (i < n) {
    dp[i] = (k - 1) * (dp[i - 2] + dp[i - 1]);
    i++;
  }

  // 这里 dp[n - 1] 也已经代表了总数量 
  return dp[n - 1] ;
}
{% endcodeblock %}

### Solution3
动态规划

用爬楼梯那道题的节约空间的方式
{% codeblock lang:java  %}
public int numWays(int n, int k) {
  if (n <= 0 || k <= 0) {
    return 0;
  }

  if (n == 1) {
    return k;
  }

  int[] dp = new int[3];

  dp[0] = k;
  dp[1] = k * k;

  int i = 2;
  while (i < n) {
    dp[i % 3] = (k - 1) * (dp[(i - 1) % 3] + dp[(i - 2) % 3]);
    i++;
  }


  return dp[(n - 1) % 3] ;
}
{% endcodeblock %}
*** 

Over！










































