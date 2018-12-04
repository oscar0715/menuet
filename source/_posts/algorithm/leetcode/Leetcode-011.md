---
title: Leetcode_011
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-28 19:08:45
---
Container With Most Water
<!-- more -->

***

### Problem
Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container.
 
### Solution
要点：
1. 从两头分别开始走
2. 选择短的那头向里靠拢

{% codeblock lang:java  %}
public int maxArea(int[] height) {
    int length = height.length;
    int i = 0;
    int j = length-1;

    int sMax = 0;
    for (; i < j; ) {
        if (height[i]<height[j]) {
            sMax = Math.max(sMax, height[i]*(j-i));
            i++;
        }else{
            sMax = Math.max(sMax, height[j]*(j-i));
            j--;
        }
    }
    return sMax;
}

{% endcodeblock %}
