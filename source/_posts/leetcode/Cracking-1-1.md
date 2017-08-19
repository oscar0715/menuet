---
title: Cracking the Code Interview 1.1
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-16 09:59:00
---
Is Unique

<!-- more -->

***

### Difference ASCII / Unicode
ASCII defines 128 characters, which map to the numbers 0–127. Unicode defines (less than) 221 characters, which, similarly, map to numbers 0–221 (though not all numbers are currently assigned, and some are reserved).

Unicode is a superset of ASCII, and the numbers 0–128 have the same meaning in ASCII as they have in Unicode. For example, the number 65 means "Latin capital 'A'".

Because Unicode characters don't generally fit into one 8-bit byte, there are numerous ways of storing Unicode characters in byte sequences, such as UTF-32 and UTF-8.

In extended ASCII, Assume 256 Characters.

### Solution 
1. Ask if the string is a unicode string or ASCII string
2. Assume it is an ASCII string
{% codeblock lang:java  %}
public boolean isUnique(String s) {
	// ASCII defines 128 character
	if (s.length() > 128) {
		return false;
	}
	boolean[] charSet = new boolean[128];

	for (int i = 0; i < s.length(); i++) {
		int val = s.charAt(i);
		if (charSet[val]) {
			return false;
		}
		charSet[val] = true;
	}
	return true;
}
{% endcodeblock %}



***

Over！
