---
title: Leetcode_022
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-17 23:26:45
---
Generate Parentheses
<!-- more -->

***

### Problem
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

	[
	  "((()))",
	  "(()())",
	  "(())()",
	  "()(())",
	  "()()()"
	]

### Solution

要点：
1. 只要"("的数量没有超过n，都可以插入"("。
2. 而可以插入")"的前提则是当前的"("数量必须要多余当前的")"数量。

{% codeblock lang:java  %}
public List<String> generateParenthesis(int n) {
	List<String> result = new ArrayList<String>();
	if(n <= 0){
		return result;
	}
	
	helper(result, "", n, n);
	return result;
		
}

public void helper(List<String> result, String present, int left, int right) {
	if(right == 0) {
		result.add(present);
		return;
	}
	
	if(left > 0) {
		helper(result, present + "(", left - 1, right);
	}
	
	// "(" left is smaller than ")" left
	if(left < right) {
		helper(result, present + ")", left, right - 1);
	}
}
{% endcodeblock %}
