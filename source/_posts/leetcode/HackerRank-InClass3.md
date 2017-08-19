---
title: HackerRank Map Sort
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-06 13:21:0
---
对Map的Key 和 value 排序
<!-- more -->

***

### Problem
1. 给一个`String[]` 
`String[] votes = { "Alex", "Michael", "Harry", "Dave", "Michael","Victor", "Harry", "Alex", "Mary", "Mary", }`
2. 选出票数最多的，得票相同的情况下，字母排序考后的胜利

### Solution 

{% codeblock lang:java  %}
static String electionWinner(String[] votes) {
	// 对key排序
	Map<String, Integer> map = new TreeMap<String, Integer>(new Comparator<String>() {
		public int compare(String o1, String o2) {
			return o2.compareTo(o1);
		}
	});

	for (String s : votes) {
		if (map.containsKey(s)) {
			int num = map.get(s);
			map.put(s, num + 1);
		} else {
			map.put(s, 1);
		}
	}

	// 把entry装到List里
	List<Map.Entry<String, Integer>> list = new ArrayList<Map.Entry<String, Integer>>(map.entrySet());

	// 对value排序
	Collections.sort(list, new Comparator<Map.Entry<String, Integer>>() {
		public int compare(Map.Entry<String, Integer> o1,
								  Map.Entry<String, Integer> o2) {
			return o2.getValue() - o1.getValue();
		}
	});

	return list.get(0).getKey();

}
{% endcodeblock %}

