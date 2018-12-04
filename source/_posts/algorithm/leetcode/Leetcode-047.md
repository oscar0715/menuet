---
title: Leetcode_047
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2017-03-23 19:12:00
---
Permutations II
<!-- more -->

***

### Problem
给定一个数组，给出数组元素的所有排列方式。
数组中元素有可能重复。

For example,
[1,1,2] have the following unique permutations:
```
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

### Solution
backtracking
其实也就是

代码如下
``` java
public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        
        // 排序 方便后面去重    
        Arrays.sort(nums);
        
        boolean[] flag = new boolean[nums.length];
        backtrack(res, new ArrayList<>(), nums, flag);

        return res;
        
    }
    
    public void backtrack(List<List<Integer>> res, List<Integer> list, int[] nums, boolean[] flag) {
        if(list.size() == nums.length) {
            // 全部元素都添加了
            // 把当前lits添加到结果集中
            res.add(list);
        } else {
            for(int i = 0; i < nums.length; i++) {
                if(flag[i]) {
                    continue;
                }
                // 这个很厉害
                // 如果前面那个没有添加过，就continue
                // 意味着什么呢？
                // 因为前面的sort，重复的元素会连在一起
                // 那么只有一种可能，相同元素只能按照顺序被添加进来。
                //（就是所有相同元素的相对顺序都是一样的）
                // 和别的元素的绝对顺序可能不一样。
                // 那些乱序的情况都被continue了。
                if(i > 0 && nums[i] == nums[i-1] && !flag[i-1]){
                    continue;   
                }
                
                list.add(nums[i]);
                flag[i] = true;
                // 其实就是dfs
                backtrack(res, list, nums, flag);
                
                list.remove(list.size()-1);
                flag[i] = false;
            }
        }
    }
}

```


*** 

Over！


































