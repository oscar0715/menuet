#---
title: Leetcode_290
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-03 00:28:00
---
Nim Game

<!-- more -->

***

### Problem
Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.

	Examples:
	pattern = "abba", str = "dog cat cat dog" should return true.
	pattern = "abba", str = "dog cat cat fish" should return false.
	pattern = "aaaa", str = "dog cat cat dog" should return false.
	pattern = "abba", str = "dog dog dog dog" should return false.

Notes:
You may assume pattern contains only lowercase letters, and str contains lowercase letters separated by a single space.

### Solution
1. 把pattern的每一个character放到map里，value是位置的list
2. 然后用list里每一个位置的string去比较

还是太复杂了

{% codeblock lang:java  %}
public boolean wordPattern(String pattern, String str) {
	if(pattern.length() != str.split(" ").length){
		return false;
	}
	Map<Character, List<Integer>> patternMap = new HashMap<Character, List<Integer>>();

	char[] patternArray = pattern.toCharArray();

	for(int i = 0; i < patternArray.length; i++) {
		if(patternMap.containsKey(patternArray[i])){
			patternMap.get(patternArray[i]).add(i);
		} else {
			List<Integer> list = new ArrayList<Integer>();
			list.add(i);
			patternMap.put(patternArray[i], list);
		}
	}

	String[] strArray = str.split(" ");
		
		Set<String> set = new HashSet<String>();
		
	for(Character c : patternMap.keySet()){
		List<Integer> list = patternMap.get(c);
		String word = strArray[list.get(0)];
		if(set.contains(word)) {
			return false;
		} else {
			set.add(word);
		}
		for(int i = 1; i < list.size(); i++){
			if(!strArray[list.get(i)].equals(word)) {
				return false;
			}
		}
	}

	return true;
}
{% endcodeblock %}

### Solution2
比人家的做法，屌爆了

1. map的put会返回一个旧值，如果没有旧值，那返回null
2. 同一个map同时维护了pattern 和 words，value都是位置
3. 判断的条件是，相同的pattern的上一个位置是不是相同
4. 如果上一个位置不同，那么就是false

{% codeblock lang:java  %}
public boolean wordPattern(String pattern, String str) {
	String[] words = str.split(" ");

	if (words.length != pattern.length()) {
		return false;
	}

	Map index = new HashMap();

	for (Integer i = 0; i < words.length; ++i) {
		if (index.put(pattern.charAt(i), i) != index.put(words[i], i)) {
			return false;
		}
	}
	return true;
}
{% endcodeblock %}

*** 

Over！










































