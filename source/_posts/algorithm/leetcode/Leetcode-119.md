---
title: Leetcode_119
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-08 23:30:00
---
Pascal's Triangle II

<!-- more -->

***

### Problem
<pre>
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
</pre>


Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].


### Solution 简单易懂版
就是118题稍微改动一下。
{% codeblock lang:java  %}
public List<Integer> getRow(int rowIndex) {
 
  List<Integer> row = new ArrayList<Integer>();
  
  row.add(1);

  for (int i = 1; i < rowIndex+1; i++) {
    List<Integer> newRow = new ArrayList<Integer>();
    newRow.add(1);
    for (int j = 1; j < row.size(); j++) {
      newRow.add(row.get(j - 1) + row.get(j));
    }
    newRow.add(1);
    row = newRow;
  }
  return row;

}
{% endcodeblock %}

*** 

Over！










































