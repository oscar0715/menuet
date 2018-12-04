---
title: Leetcode_026
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-01 13:10:45
---
Remove Duplicates from Sorted Array
数组
<!-- more -->

***

### Problem
Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array `nums = [1,1,2]`,

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

### Solution

题目的意思是，返回一个长度n
然后nums的前n个值就是无重复的数组，是去掉重复数字的结果

要点：
1. 通常的感觉就是，如果重复了以后，要把后面剩下的数组整体迁移去覆盖。其实根本不用的。
2. 本质上也是应用了快慢指针的思想
3. i 指针走在前面，指向最新的一个数
4. current指针指向现在没有重复的最新的一个数

{% codeblock lang:java  %}
public int removeDuplicates(int[] nums) {
    if (nums.length <= 1) {
        return nums.length;
    }

    int current = 0;
    for (int i = 1; i < nums.length; i++) {
        if (nums[current] != nums[i]) {
            current++;
            // 更新最新的数值到当前位置
            // 如果之前没有发生重复过，那么current==i
            // 如果之前发生重复过，那么当前current的值一定已经被i走过了，所以这个位置上的数值是不重要的。
            nums[current] = nums[i];
        }
    }

    // 在i走完以后，current肯定在无重复数组的最后一个位置，数组的长度为位置+1；
    return current++;
}
{% endcodeblock %}
















