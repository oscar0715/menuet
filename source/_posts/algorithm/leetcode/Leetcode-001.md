---
title: Leetcode_001
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-06-23 09:59:00
---
Two Sum

<!-- more -->

***

### Problem
给定一个数组和一个目标数字，从数组中找两个数字使它们的和为这个目标数字
。

1. 暴力破解 $O(n^2)$ 复杂度
2. 排序后，从两头向中间找 $O(nlogn)$
3. 用map的 containskey 方法

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

Example:
{% codeblock %}
	Given nums = [2, 7, 11, 15], target = 9,
	Because nums[0] + nums[1] = 2 + 7 = 9,
	return [0, 1].
{% endcodeblock %}

UPDATE (2016/2/13):
The return format had been changed to zero-based indices. Please read the above updated description carefully.



### Solution 初级入门版
当然是很慢的那种
{% codeblock lang:java  %}
   
	public int[] twoSum(int[] nums, int target) {
		int first = 0;
		int second = first + 1;
		int size = nums.length;
		
		for (; first < size; first++) {
			for (second=first+1; second < size; second++) {
				if ((nums[first] + nums[second]) == target) {
					int[] result= {first,second} ;
					return result;
				}
			}
		}
		return null;
	}
{% endcodeblock %}

### Solution 提升版
{% codeblock lang:java  %}
   
	public int[] twoSum(int[] nums, int target) {

		int[] numbers = nums.clone(); 
		Arrays.sort(nums); 

		int left = 0;    
		int right = nums.length - 1; 

		int[] ans = { 0, 0 };
		int sum = 0;

		ArrayList<Integer> index = new ArrayList<Integer>();

		while (left < right) {
			sum = nums[left] + nums[right];

			if (sum < target) {
				// 当sum小于target，只能是left++，因为right是从最大数移过来的
				left++; 
			} else if (nums[left] + nums[right] > target) {
				// 反之亦然
				right--;
			} else {
				// 到number里找原来的顺序
				for (int i = 0; i < numbers.length; i++) {
					if (numbers[i] == nums[left]) {
						index.add(i);
					} else if (numbers[i] == nums[right]) {
						index.add(i);
					} else if (index.size() == 2) {
						break;
					}
				}
				break;
			}
		}
		ans[0] = index.get(0);
		ans[1] = index.get(1);
		return ans;
	}
{% endcodeblock %}

总结一下
1. 查找的时候，有序列表从两头开始，可以大大减少查找次数(重点是避免了嵌套的for循环)

### Solution 终极简洁HashMap版
{% codeblock lang:java  %}
public int[] twoSum(int[] nums, int target) {       
	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
	int[] result = new int[2];
	for(int i = 0; i < nums.length; i++) {
		if(map.containsKey(target- nums[i])) {
			result[0] = map.get(target - nums[i]);
			result[1] = i;
			return result;
		} else {
			map.put(nums[i], i);
		}
	}
	return result;
}
{% endcodeblock %}

***

Over！
