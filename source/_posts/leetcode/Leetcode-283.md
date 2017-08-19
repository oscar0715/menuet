---
title: Leetcode_283
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-09 23:40:00
---
Move Zeroes

<!-- more -->

***

### Problem
Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.


### Solution 一种思路
1. 时间复杂度O(n)
2. 前后指针思路，slow为零，则与下一个非零交换

{% codeblock lang:java  %}
public void moveZeroes(int[] nums) {
    if (nums.length <= 1) {
      return;
    }
    int fast = 0;
    int slow = 0;

    for (; slow < nums.length&&fast<nums.length; slow++,fast++) {
      if (nums[slow] == 0) {

        while (nums[fast] == 0) {

          fast++;
          if (!(fast < nums.length)) {
            return;
          }

        }
        nums[slow] = nums[fast];
        nums[fast] = 0;
        
      }
      

    }

  }
{% endcodeblock %}



### Solution 另一种思路
1. 同样前后指针
2. 把所有非零的值赋到前面
3. 同一个数组空间内的复制

更快！！！

{% codeblock lang:java  %}

public void moveZeroes(int[] nums) {
    int cnt = 0, pos = 0;
    // 将非0数字都尽可能向前排
    for(int i = 0; i < nums.length; i++){
        if(nums[i] != 0){
            nums[pos]= nums[i];
            pos++;
        }
    }
    // 将剩余的都置0

    for(;pos<nums.length; pos++){
        nums[pos] = 0;
    }
}

{% endcodeblock %}
*** 

Over！










































