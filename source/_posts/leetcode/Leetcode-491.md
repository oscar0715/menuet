---
title: Leetcode_491
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-22 22:49:00
---
Increasing Subsequences

<!-- more -->

***

### Problem
给定一个数组，给出所有增序的子数组，可以跳过一些元素。

Example:
Input: [4, 6, 7, 7]
Output: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]

### Solution DFS

两层循环

1. 第一层，每个元素都当做起点
2. 第二层，DFS 思想。以初始传入的数组位置为起点，只要有增序的就添加入 res 集合。

``` java
public class Solution {
    Set<List<Integer>> res = new HashSet<>();
    
    public List<List<Integer>> findSubsequences(int[] nums) {
        
        if(nums.length < 2){
            return new ArrayList<>();     
        }
        
        // 第一层循环
        for(int i = 0; i < nums.length; i++) {
            chechIncreasing(nums, i, new ArrayList<>());
        }
        
        return new ArrayList<>(res);
    }

    
    // 第二层递归，运用DFS的思想
    public void chechIncreasing(int[] nums, int point, List<Integer> list) {
        if(point < nums.length) {
            if(list.isEmpty()){
                // 空list时，添加当前元素后，直接下一个递归
                list.add(nums[point]);
                chechIncreasing(nums, point + 1, new ArrayList<>(list));
            } else {
                // 跳过当前元素，递归
                chechIncreasing(nums, point + 1, new ArrayList<>(list));

                // 如果当前元素符合增序，添加后，将当前list添加到res,然后进行下一轮递归
                if ( nums[point] >= list.get(list.size() - 1) ) {
                    list.add(nums[point]);
                    res.add(list);
                    chechIncreasing(nums, point + 1, new ArrayList<>(list));
                } 
            }
        }
    }
}
```











































