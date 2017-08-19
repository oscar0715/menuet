---
title: Leetcode_532
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-10 20:55:00
---
K-diff Pairs in an Array

<!-- more -->

***

# Problem
给定一个乱序，可能重复的数组，给定一个数字k，统计数组中差值为k的数字组合的数量

# Solution 
第一种方法 用 HashMap
``` java
public class Solution {
	public int findPairs(int[] nums, int k) {
		
		if ( k < 0) {
			return 0;
		}
		
		if (nums.length < 2) {
			return 0;
		}
		
		// put all elements into a map
		Map<Integer, Integer> map = new HashMap<>();
		for(Integer i : nums) {
			if( map.containsKey(i)) {
				map.put(i,map.get(i) + 1);
			} else {
				map.put(i,1);
			}	
		}
		
		int count = 0;
		
		if ( k == 0 ){
			// if k == 0
			// then test if there is duplicate elements
			for( Integer i : map.keySet()){
				if(map.get(i) > 1) {
					count ++;
				}
			} 
		} else {
			// otherwise ...
			for( Integer i : map.keySet()){
				if(map.containsKey(i - k)) {
					count++;
				}
			}
		}
		
		return count;
	}
}
```


