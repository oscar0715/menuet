---
title: Leetcode_344
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-10 14:40:00
---
Reverse String

<!-- more -->

***

### Problem
Write a function that takes a string as input and returns the string reversed.

Example:
Given s = "hello", return "olleh".


### Solution 一种思路
用Stringbuilder的reverse方法
空间上多了一个Stringbuilder（不知道背后怎么实现，懒得看源码了）
``` java
  public String reverseString(String s) {
    if (s.length()<2) {
      return s;
    }
    
    StringBuilder result = new StringBuilder(s);
    return result.reverse().toString();
  }
```



### Solution 另一种思路
这种自己实现，快一点
空间上多了 两个char[]数组

``` java
public String reverseString(String s) {
  if (s.length()<2) {
    return s;
  }
  
  char[] string = s.toCharArray();
  char[] arr = s.toCharArray();
  int i = s.length()-1;
  for(char a : string){
    arr[i] = a;
    i--;
  }
  return String.valueOf(arr);
  
}
```

### Solution 还有一种思路
还有更快的
``` java
public String reverseString(String s) {
  if (s.length() < 2) {
    return s;
  }

  char[] string = s.toCharArray();
  
  int head = 0;
  int end = string.length-1;
  
  // 循环只要走n/2的长度
  while (head < end) {
    char tmp = string[head];
    string[head] = string[end];
    string[end] = tmp;
    head++;
    end--;
  }
  return new String(string);
}
```



*** 

Over！










































