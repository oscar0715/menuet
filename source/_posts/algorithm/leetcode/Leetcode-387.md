---
title: Leetcode_387
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-08-27 16:23:00
---
First Unique Character in a String

<!-- more -->

***

### Solution 
送分题
1. 用一个长度为256的int数组来记录字符串中字母出现过的次数
2. 再走一遍字符串，返回第一个次数为1的字符的位置
3. 如果没有则返回-1

``` java
public int firstUniqChar(String s) {
	int[] chars = new int[256];
	
	for(int i = 0; i < s.length(); i++) {
		chars[s.charAt(i)]++;
	}
	
	for(int i = 0; i < s.length(); i++) {
		if(chars[s.charAt(i)] == 1) 
			return i;
	}
	
	return -1;
}
```











































