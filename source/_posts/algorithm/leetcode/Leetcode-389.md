---
title: Leetcode_389
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-01 13:23:00
---
Find the Difference

<!-- more -->

***

### Problem
Given two strings s and t which consist of only lowercase letters.

String t is generated by random shuffling string s and then add one more letter at a random position.

Find the letter that was added in t.

Example:

	Input:
	s = "abcd"
	t = "abcde"

	Output:
	e

	Explanation:
	'e' is the letter that was added.

### Solution 
送分题

{% codeblock lang:java  %}
public char findTheDifference(String s, String t) {
	char[] sArray = s.toCharArray();
	char[] tArray = t.toCharArray();
	Arrays.sort(sArray);
	Arrays.sort(tArray);

	int i = 0;
	for( ; i< s.length(); i++) {
		if(sArray[i] != tArray[i]) {
			return tArray[i];
		}
	}
	
	return tArray[i];
}
{% endcodeblock %}











































