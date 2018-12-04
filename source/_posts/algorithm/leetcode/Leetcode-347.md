---
title: Leetcode_347
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2016-09-01 23:48:00
---
Top K Frequent Elements

<!-- more -->

***

### Problem
Given a non-empty array of integers, return the k most frequent elements.

For example,
Given [1,1,1,2,2,3] and k = 2, return [1,2].

Note: 
You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
Your algorithm's time complexity must be better than O(n log n), where n is the array's size.


### Solution 一种思路
本质是对map的value排序

1. 搞了个新的对象
2. Collection.sort

{% codeblock lang:java  %}
public List<Integer> topKFrequent(int[] nums, int k) {
	Map<Integer, KeyObject> map = new HashMap<Integer, KeyObject>();

	for (int num : nums) {
		if (map.containsKey(num)) {
			map.get(num).addFrequency();
		} else {
			map.put(num, new KeyObject(num));
		}
	}
	List<KeyObject> list = new ArrayList<KeyObject>();
	for (KeyObject o : map.values()) {
		list.add(o);
	}
	Collections.sort(list, new Comparator<KeyObject>() {
		public int compare(KeyObject o1, KeyObject o2) {
			return o2.getFrequency() - o1.getFrequency();
		}
	});

	List<Integer> result = new ArrayList<Integer>();
	for (int i = 0; i < k && i < list.size(); i++) {
		result.add(list.get(i).getKey());
	}
	return result;
}

public class KeyObject {
	int key;
	int frequency;

	public KeyObject(int key) {
		this.key = key;
		this.frequency = 1;
	}

	public int getKey() {
		return key;
	}

	public int getFrequency() {
		return frequency;
	}

	public void addFrequency() {
		this.frequency++;
	}
}
{% endcodeblock %}

*** 

Over！










































