---
title: Leetcode_075
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-25 23:17:00
---
Sort Colors
快速排序的思想
<!-- more -->

***

### Problem
Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note:
You are not suppose to use the library's sort function for this problem.

click to show follow up.

Follow up:
A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.

Could you come up with an one-pass algorithm using only constant space?


### Solution 递归

一个解释详细的reference: {% link Leetcode 75 Sort Colors http://blog.csdn.net/zxzxy1988/article/details/8596144 %}

思路：
用三个指针，i指向1元素的开始，作为0和1的分界点。j指向1元素的结尾，作为1和2的分界点。k用来遍历。

遍历的时候采取这样的策略：
1. 如果k指向的元素为0，那么交换k和i位置的元素。
2. 因为刚刚的定义，所以我们总是能够保证交换过来的元素是1，那这样的话我们就i++,k++。
3. 如果k指向的元素是2，那么我们交换j和k指向的元素，j--。
4. 但是k不变，因为我们不知道交换过来的是什么元素。

``` java
public void sortColors(int[] nums) {
    if (nums.length < 2) {
        return;
    }

    int i = 0;
    int j = nums.length - 1;


    for (int k = 0; k <= j ; k++) {
        if (nums[k] == 0) {
            nums[k] = nums[i];
            nums[i] = 0;
            i++;
            // 此处k可以自增
            // 因为swap过来的一定是1

        } else if (nums[k] == 2) {
            nums[k] = nums[j];
            nums[j] = 2;
            j--;
            k--;
            // 此处k--, 然后自增以后保持不变
            // 因为从后面,换过来的不知道是什么
            // 要重新判断一遍
        }
    }
}
``` 


*** 

Over！










































