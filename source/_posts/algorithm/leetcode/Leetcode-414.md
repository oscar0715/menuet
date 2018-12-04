---
title: Leetcode_414
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-08 17:00:00
---
Third Maximum Number

<!-- more -->

***

### Problem
给定一个乱序数组。如果存在第三大的数，则返回第三大的数，否则返回最大的数。（重复的数字不算）

### Solution 

``` java
public class Solution {
	public int thirdMax(int[] nums) {
		Integer[] maxArray = new Integer[3];
		
		for ( Integer n : nums) {
			
			// ignore duplicate number
			if (n.equals(maxArray[0]) || n.equals(maxArray[1]) || n.equals(maxArray[2]) ) {
				continue;
			}
			
			if (maxArray[0]==null || n > maxArray[0]) {
				maxArray[2] = maxArray[1];
				maxArray[1] = maxArray[0];
				maxArray[0] = n;
			} else if (maxArray[1] == null || n > maxArray[1]) {
				maxArray[2] = maxArray[1];
				maxArray[1] = n;
			} else if (maxArray[2] == null || n > maxArray[2]){
				maxArray[2] = n;
			}
		}
		
		if (maxArray[2] == null ) {
			return maxArray[0];
		} else {
			return maxArray[2];
		}
	}
}
```











































