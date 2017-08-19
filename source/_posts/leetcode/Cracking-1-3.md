---
title: Cracking the Code Interview 1.3
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-16 09:59:00
---
URIlify

<!-- more -->

***

### Problem
replace space with `%20`

### Solution 1
String manipulation: edit string from the end. So we don't need to worry about the overwriting.

{% codeblock lang:java  %}
public boolean checkPermutation(String s1, String s2) {
	public String urlify(char[] str, int trueLength) {
		int spaceCount = 0;
		for (int i = 0; i < trueLength; i++) {
			if (str[i] == ' ') {
				spaceCount++;
			}
		}

		int index = trueLength + 2 * spaceCount - 1;

		for (int i = trueLength - 1; i >= 0; i--) {
			if (str[i] == ' ') {
				str[index--] = '0';
				str[index--] = '2';
				str[index--] = '%';
			} else {
				str[index--] = str[i];
			}
		}
		return new String(str);

	}
}
{% endcodeblock %}



***

Over！
