---
title: Cracking the Code Interview 1.7
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-10-16 19:03:00
---
Rotate matrix

<!-- more -->

***

# Problem
Rotate a N*N matrix

# Solution 
{% codeblock lang:java  %}
public static int[][] rotate(int[][] matrix) {
	int n = matrix.length - 1;
	int layer = 0;
	while (layer <= n) {
		for (int i = layer; i < n - layer; i++) {
			int first = matrix[layer][i];
			int second = matrix[i][n - layer];
			int third = matrix[n - layer][n - i];
			int fourth = matrix[n - i][layer];

			matrix[layer][i] = fourth;
			matrix[i][n - layer] = first;
			matrix[n - layer][n - i] = second;
			matrix[n - i][layer] = third;
		}
		layer++;
	}

	return matrix;
}

{% endcodeblock %}

***

Over！
