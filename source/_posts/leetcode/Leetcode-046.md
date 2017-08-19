---
title: Leetcode_046
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2017-03-23 14:41:00
---
Permutations

<!-- more -->

***

### Problem
给定一个数组，给出数组元素的所有排列方式。

For example,
[1,2,3] have the following permutations:
```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

### Solution
最简单的 backtracking 了吧。

代码如下
``` java
public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        
        backtrack(res, new ArrayList<>(), nums);
        
        return res;
    }
    
    public void backtrack(List<List<Integer>> res, List<Integer> current, int[] nums ){
        if(current.size() == nums.length) {
            res.add(current);
        } else {
            for(int i = 0; i < nums.length; i++) {
                if(current.contains(nums[i])){
                    continue;
                } else {
                    current.add(nums[i]);
                    // 注意这个地方是不用新建一个list对象，传入的
                    // 因为递归出来以后反正会remove掉的
                    backtrack(res, current, nums);
                    // 后退一步
                    current.remove(current.size()-1);
                }
            }
        }
    }
}
```


*** 

Over！



















































