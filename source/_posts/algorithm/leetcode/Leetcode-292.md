---
title: Leetcode_292
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-13 21:40:00
---
Nim Game

<!-- more -->

***

### Problem
Nim Game

有总数为n的石头，每个人可以拿1~3个石头，两个人交替拿，拿到最后一个的人获胜。究竟是先手有利，还是后手有利。

	1个石子，先手全部拿走；
	2个石子，先手全部拿走；
	3个石子，先手全部拿走；
	4个石子，后手面对的是先手的第1，2，3情况，后手必胜；
	5个石子，先手拿走1个让后手面对第4种情况，后手必败；
	6个石子，先手拿走2个让后手面对第4种情况，后手必败；

1. 只要总数为4的倍数，先手必败。
先手拿n个，后手拿4-n个，最后会剩下4个，先手败。
2. 总数不为4的倍数，后手必败
先手每次拿完石头，使剩下石头总数为4的倍数即可。


### Solution

{% codeblock lang:java  %}
public boolean canWinNim(int n) {
	return n%4 != false;
}
{% endcodeblock %}


*** 

Over！










































