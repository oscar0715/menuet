---
title: Leetcode_049
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2016-09-01 17:22:00
---
Group Anagrams
<!-- more -->

***

### Problem
Given an array of strings, group anagrams together.

For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"], 
Return:

	[
		["ate", "eat","tea"],
		["nat","tan"],
		["bat"]
	]
Note: All inputs will be in lower-case.

### Solution
1. 先把每个字符串排序得到sortString
2. 创建一个map
3. 用sortString当key
4. value为所有anagram可以变为sortString的String的list


代码如下
``` java  
public List<List<String>> groupAnagrams(String[] strs) {
	List<List<String>> result = new ArrayList<>();
	
	Map<String, List<String>> map = new HashMap<>();
	
	for (String s : strs) {
		
		char[] charArray = s.toCharArray();
		Arrays.sort(charArray);
		String sortString = String.copyValueOf(charArray);
		
		if(map.containsKey(sortString)) {
			map.get(sortString).add(s);
		} else {
			List<String> list = new ArrayList<String>();
			list.add(s);
			map.put(sortString, list);
		}
	}
	
	for (List<String> list : map.values()) {
		result.add(list);
	}
	
	return result;
}

```


*** 

Over！


































