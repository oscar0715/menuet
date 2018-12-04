---
title: Leetcode_027
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-02 11:10:45
---
Remove Element
数组
<!-- more -->

***

### Problem
Given an array and a value, remove all instances of that value in place and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

### Solution

题目的意思是，返回一个长度n
然后nums的前n个值就是去掉给定值以后的数组

要点：
1. 思想上和上一题是一样一样的
2. 本质上也是应用了快慢指针的思想

我自己的写的 慢板
{% codeblock lang:java  %}
public int removeElement(int[] nums, int val) {

        if (nums.length == 0) {
            return 0;
        }

        int slow = 0;
        int fast = 1;
        for (; fast < nums.length;) {
            
            if (val == nums[slow]) {
                // 如果nums[slow]和val相等
                // 那么先把fast的值copy过来
                nums[slow] = nums[fast];
                // 然后把fast的值赋为val，这样这个位置之后就会空出来了 
                nums[fast] = val;
                fast++;
            } else {
                不相等的话，slow这个位置检验通过
                slow++;
            }
        }
        if (nums[slow] == val) {
            slow--;
        }
        return ++slow;
    }
{% endcodeblock %}

别人写的 快板
{% codeblock lang:java  %}
public int removeElement(int[] nums, int val) {
    if (nums.length == 0) {
        return 0;
    }

    int current = -1;
    int i = 0;
    for (; i < nums.length; i++) {
        // 人家是判断快的那个指针
        if (nums[i] != val) {
            // 如果检验通过
            // 把值赋给慢指针的位置，慢指针总是通过以后才会用到
            current++;
            nums[current] = nums[i];
        }
    }
    return current + 1;
}
{% endcodeblock %}

用这个数组为例{ 1, 1, 2, 2, 5 }
我的慢板走6次循环
人家的快板走5次循环

结论：用块指针判断，不会拖泥带水

*** 

Over！

















