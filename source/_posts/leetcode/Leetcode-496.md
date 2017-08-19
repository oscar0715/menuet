---
title: Leetcode_496
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-29 14:46:00
---
Next Greater Element I

<!-- more -->

***

### Problem
给定两个数组 
例如：`nums1 = [4,1,2], nums2 = [1,3,4,2]`.
nums1 是 nums2 的子集，nums2 中所有数字都唯一，找出 nums 中每个元素，在 nums2 中的下一个比自己大的数字。
`Output: [-1,3,-1]`

### Solution

用 Stack 维护一个递减子序列

``` java
public class Solution {
    public int[] nextGreaterElement(int[] findNums, int[] nums) {
        
        Map<Integer, Integer> map = new HashMap<>();
        Stack<Integer> stack = new Stack<>();
        
        for(int num : nums) {
            while(!stack.isEmpty() && stack.peek() < num)
                // 不断弹出栈中比当前数字小的元素
                // 弹出来的元素都可以构成都是符合要求的键值对
                map.put(stack.pop(), num);
            // 直到栈顶元素比自己大了，或者空栈了
            // 再把自己push进去，这样保证stack是一个倒序的。    
            stack.push(num);
        }
        
        for(int i = 0; i < findNums.length; i++) {
            findNums[i] = map.getOrDefault(findNums[i], -1);
        }
        return findNums;
    }
}
```











































