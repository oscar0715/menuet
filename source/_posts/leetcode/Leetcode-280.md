---
title: Leetcode_280
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-26 21:32:00
---
Wiggle Sort
摆动排序

<!-- more -->

***

### Problem
Given an unsorted array nums, reorder it in-place such that nums[0] <= nums[1] >= nums[2] <= nums[3]....

For example, given nums = [3, 5, 2, 1, 6, 4], one possible answer is [1, 6, 2, 5, 3, 4].


### Solution 一种思路
1. 先快速排序，得到 {1, 2, 3, 4, 5, 6}
2. 更换2，3位置
3. 更换4，5位置
4. 以此类推

这种思路要2 pass
{% codeblock lang:java  %}
public void wiggleSort(int[] nums) {
    if (nums.length < 2) {
        return;
    }
    
    quickSort(nums, 0 , nums.length - 1);
    
    for (int i = 2; i < nums.length; i = i + 2) {
        int temp = nums[i];
        nums[i] = nums[i - 1];
        nums[i - 1] = temp;
    }
}
  
public void quickSort(int[] nums, int left, int right) {
    if (left < right) {
        int i = left;
        int j = right;
        
        int target = nums[left];
        
    
        while (i < j) {
            while (i < j && target < nums[j]) {
                j--;
            }
            
            if (i < j) {
                nums[i] = nums[j];
                i++;
            }
            
            while (i < j && target > nums[i]) {
                i++;
            }
            
            if (i < j) {
                nums[j] = nums[i];
            }
        }
        
        nums[i] = target;
        quickSort(nums, left, i - 1);
        quickSort(nums, i + 1, right);
    }
}
{% endcodeblock %}



### Solution 另一种思路

精致的 one pass

1. 根据i位置的奇偶性判断
2. 当i为奇数时，如果nums[i] < nums[i - 1]，则两数交换
3. 当i为偶数时，如果nums[i] > nums[i - 1]，则两数交换
4. 一次遍历达到要求

{% codeblock lang:java  %}
public void wiggleSort(int[] nums) {
    if (nums.length < 2) {
        return;
    }
    
    for (int i = 1; i < nums.length; i++) {
        if ( (i % 2 == 1 && nums[i] < nums[i - 1]) || (i % 2 == 0 && nums[i] > nums[i-1]) ) {
            int tmp = nums[i];
            nums[i] = nums[i - 1];
            nums[i - 1] = tmp;
        }
    }
}

{% endcodeblock %}

*** 

Over！










































