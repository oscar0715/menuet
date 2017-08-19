---
title: Leetcode_223
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-28 11:42:00
---
Rectangle Area

<!-- more -->

***

### Problem

find the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

两个长方形，分别给出对角的两个点，求覆盖总面积

就看有木有重复的部分。

### Solution 

别人的解法，比较聪明
{% codeblock lang:java  %}
public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H){
    int areaOfSqrA = (C-A) * (D-B);
    int areaOfSqrB = (G-E) * (H-F);
    
    int left = Math.max(A, E);
    int right = Math.min(G, C);
    int bottom = Math.max(F, B);
    int top = Math.min(D, H);
    
    //If overlap
    int overlap = 0;
    if(right > left && top > bottom)
         overlap = (right - left) * (top - bottom);
    
    return areaOfSqrA + areaOfSqrB - overlap;
}
{% endcodeblock %}


### Solution
我自己的写法，用了快速排序。
其实……别人用的max本质也是比较嘛

{% codeblock lang:java  %}
public int computeArea(int A,
                              int B,
                              int C,
                              int D,
                              int E,
                              int F,
                              int G,
                              int H) {
    int[] horizontal = { A, C, E, G };
    int[] vertical = { B, D, F, H };

    int overlap = 0;

    if (E >= C || G <= A || F >= D || H <= B) {
        return (C - A) * (D - B) + (G - E) * (H - F);
    } else {
        quickSort(horizontal, 0, 3);
        quickSort(vertical, 0, 3);

        overlap = (horizontal[2] - horizontal[1]) * (vertical[2]
                                                             * vertical[1]);

    }
    return (C - A) * (D - B) + (G - E) * (H - F) - overlap;
}

public void quickSort(int[] nums, int left, int right) {
    if(left < right) {
        
        int i = left;
        int j = right;
        
        int target = nums[left];
        
        while(i < j) {
            while (i < j && target < nums[j] ) {
                j--;
            }
            
            if(i < j) {
                nums[i] = nums[j];
                i++;
            }
            
            while (i < j && target > nums[i] ) {
                i++;
            }
            
            if(i < j) {
                nums[j] = nums[i];
                j--;;
            }
        }
        
        nums[i] = target;
        quickSort(nums, left, i - 1);
        quickSort(nums, i + 1, right);
    }
}
{% endcodeblock %}

*** 

Over！










































