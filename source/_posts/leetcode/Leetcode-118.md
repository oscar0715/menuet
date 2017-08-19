---
title: Leetcode_118
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-08 20:08:00
---
Pascal's Triangle

<!-- more -->

***

### Problem

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return
<pre>
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
</pre>


### Solution 

{% codeblock lang:java  %}
public List<List<Integer>> generate(int numRows) {
    
    List<List<Integer>> outer = new ArrayList<List<Integer>>();
    if (numRows == 0) {
      return outer;
    }

    // 规律就是利用上一行，来求下一行
    
    // generate第一行
    List<Integer> lastList = new ArrayList<Integer>();
    lastList.add(1);
    outer.add(lastList);
    
    // 循环从第二行开始
    for (int i = 1; i < numRows; i++) {
      List<Integer> inner = new ArrayList<Integer>();
      
      // 前后两个1在内层循环外添加
      inner.add(1);
      
      // 内层循环根据上一行generate当前行
      for (int j = 1; j < lastList.size(); j++) {
        inner.add(lastList.get(j - 1) + lastList.get(j));
      }
      
      inner.add(1);
      
      outer.add(inner);
      lastList = inner;
    }
    return outer;

  }
{% endcodeblock %}

*** 

Over！










































