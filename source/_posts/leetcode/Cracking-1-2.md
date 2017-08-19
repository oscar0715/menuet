---
title: Cracking the Code Interview 1.2
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-16 09:59:00
---
Chech Permutation

<!-- more -->

***

### Problem
1. Ask if the permutation is case sensitive. (For now, we assmue it is different). If not case sensitive, use `lowerCase()` function.
2. Ask is white space is significant.

### Solution 1
1. Sort the String
{% codeblock lang:java  %}
public boolean checkPermutation(String s1, String s2) {
	if (s1.length() != s2.length()) {
		return false;
	}

	return sort(s1).equals(s2);
}

String sort(String s) {
	char[] array = s.toCharArray();
	java.util.Arrays.sort(array);
	return array.toString();
}
{% endcodeblock %}

### Solution 2
1. Sort the String
{% codeblock lang:java  %}
public boolean permutation(String s, String t) {
	if (s.length() != t.length()) {
		return false;
	}

	int[] letter = new int[128];

	for (int i = 0; i < s.length(); i++) {
		// add
		char val = s.charAt(i);
		letter[val]++;
	}

	for (int i = 0; i < t.length(); i++) {
		// minus
		char val = t.charAt(i);
		letter[val]--;

		if (letter[val] < 0) {
			return false;
		}
	}
	
	return true;
}
{% endcodeblock %}

***

Over！
