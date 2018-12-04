---
title: Cracking the Code Interview 1.5
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-16 16:40:00
---

String Compresstion.

<!-- more -->

***

### Problem
1. insert
2. remove
3. replace

chech if one operation can make two strings same.

### Solution 1
1. remove is the reverse operation of insert.

{% codeblock lang:java  %}
public boolean oneAway(String s, String t) {
	if (s.length() == t.length()) {
		return replceCheck(s, t);
	} else if (s.length() - t.length() == 1) {
		return insertCheck(t, s);
	} else if (t.length() - s.length() == 1) {
		return insertCheck(s, t);
	} else {
		return false;
	}
}

boolean replceCheck(String s, String t) {
	boolean diffrence = false;
	for (int i = 0; i < s.length(); i++) {
		if (s.charAt(i) != t.charAt(i)) {
			if (diffrence) {
				return false;
			}
			diffrence = true;
		}
	}
	return true;
}

boolean insertCheck(String shortStr, String longStr) {
	int indexShort = 0;
	int indexLong = 0;
	while (indexLong < longStr.length() && indexShort < shortStr.length()) {
		if (shortStr.charAt(indexShort) == longStr.charAt(indexLong)) {
			indexLong++;
			indexShort++;
		} else {
			if (indexLong == indexShort ) {
				indexLong++;
			} else {
				return false;
			}
		}
	}
	return true;
}
{% endcodeblock %}



***

Over！
