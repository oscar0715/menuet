---
title: Leetcode_346
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-10 14:40:00
---
Moving Average from Data Stream

<!-- more -->

***

### Problem
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3


### Solution 一种思路
简单的思路
1. 用queue pop,add 方便
2. size 定义了窗口大小
3. count 计数

缺点就是不够快，应该是queue的问题
{% codeblock lang:java  %}
public class MovingAverage {

	/** Initialize your data structure here. */
	Queue<Integer> queue = new LinkedList<Integer>();
	int size = 0;
	int sum = 0;
	int count = 0;
  
public MovingAverage(int size) {
	this.size = size;
	}
	
public double next(int val) {
	if (queue.size()==size) {
	  sum = sum - queue.poll();
	}else {
	  count++;
	}
		queue.add(val);
		sum = sum+val;
		return sum*1.0/count;
	}
}
{% endcodeblock %}



### Solution 另一种思路

数组版，但是没有比较快，好奇怪
复用一个数组

精髓在于 (++position)%size
{% codeblock lang:java  %}
public class MovingAverage {

	int size = 0;
	int sum = 0;
	int count;

	int[] numbers;
	int position = 0;

	public MovingAverage(int size) {
		this.size = size;
		numbers = new int[size];

	}

	public double next(int val) {
		if (count == size) {
			sum = sum - numbers[position];
			
		} else {
			count++;
		}
		numbers[position] = val;
		position = (position+1) % size;
		sum = sum + val;

		return sum * 1.0 / count;
	}
}
{% endcodeblock %}



*** 

Over！










































