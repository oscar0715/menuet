---
title: Leetcode_274
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2016-08-26 15:07:00
---
H-Index

<!-- more -->

***

# Problem
计算H-index

For example, given citations = [3, 0, 6, 1, 5], which means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively. Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, his h-index is 3.

Note: If there are several possible values for h, the maximum one is taken as the h-index.

# Solution
思路：
1. 按被引次数从高到低排列
2. 最后一篇 引用次数大于等于序号的 论文的序号 就是答案

``` java
public int hIndex(int[] citations) {
	int length = citations.length;

	// 从大到小 快速排序
	quickSort(citations, 0, length - 1);

	int result = 0;

	// 从最高的第1篇开始核对，数组里还是0开始所以是 i - 1 
	for (int i = 1; i <= length && citations[i - 1] >= i; i++) {
			result = i;
	}
	return result ;
}

// 从大到小 排序
public void quickSort(int[] nums, int left, int right) {
	if (left < right) {
		int i = left;
		int j = right;

		int target = nums[i];

		while (i < j) {
			while (i < j && target > nums[j]) {
				j--;
			}

			if (i < j) {
				nums[i] = nums[j];
				i++;
			}

			while (i < j && target < nums[i]) {
				i++;
			}

			if (i < j) {
				nums[j] = nums[i];
				j--;
			}

		}

		nums[i] = target;
		quickSort(nums, left, i - 1);
		quickSort(nums, i + 1, right);
	}
}
```

# 数组从小到大快速排序注释
``` java
// 从小到大排序
private void quickSort(int[] nums, int left, int right) {
	if(left < right) {
		int i = left;
		int j = right;
		
		// 挖个坑, 在位置 i，留给比 target 大的数
		int target = nums[i];
		
		while( i < j ){
			// 找到最右边的那个比target小的数
			while(i<j && target < nums[j]) {
				j--;
			}
				
				
			if(i < j){
				// 找到了一个比target 小的
				// 放到那个坑里面
				nums[i] = nums[j];
				// 坑的位置++；
				i++;
			}
			
			// 这个时候 j 变成了新的坑
			// 留给比target大的数字
			
			// 从i出发找更比target更大的数字
			while(i<j && target > nums[i]) {
				i++;
			}
			
			if(i < j) {
				// 找到了填到坑里面
				// 这个时候i
				// 又成了新的坑
				nums[j] = nums[i];
				j--;
			}
		}
		
		// 好了一轮走完了
		// 这个时候 i 位置
		// 左边比target小，右边比tagert大
		
		// 把target 填回去
		nums[i] = target;
		
		// 递归
		quickSort(nums,left, i-1);
		quickSort(nums,i+1,right);
			
	}
}
```


*** 

Over！










































