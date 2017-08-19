---
title: Leetcode_506
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-02 23:20:00
---
Relative Ranks

<!-- more -->

***

# Problem
给定一个整数数组
返回从大到小的相对位置

Example 1:
```
Input: [5, 4, 3, 2, 1]
Output: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
Explanation: The first three athletes got the top three highest scores, so they got "Gold Medal", "Silver Medal" and "Bronze Medal". 
For the left two athletes, you just need to output their relative ranks according to their scores.
```

# Solution 

``` java
public class Solution {
    public String[] findRelativeRanks(int[] nums) {
        String[] res = new String[nums.length];
        if(nums.length == 0) {
            return res;
        }
        
        // Map 记录 <Score, position>
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        
        // sort score
        Arrays.sort(nums);
        int m = nums.length - 1;
        
        // 通过顺序的score去map里找位置
        
        // 金银铜
        res[map.get(nums[m])] = "Gold Medal";
        if(m - 1 >= 0) {
            res[map.get(nums[m-1])] = "Silver Medal";
        }
        if(m-2 >= 0 ) {
            res[map.get(nums[m-2])] = "Bronze Medal";
        }
        // 第四名以后
        for(int i = m-3; i >= 0; i--) {
            res[map.get(nums[i])] = m - i + 1+"";
        }
        
        return res;
    }
}
```


