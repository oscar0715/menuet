---
title: Leetcode_046
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2017-03-23 20:49:00
---
Combinations

<!-- more -->

***

### Problem
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

For example,
If n = 4 and k = 2, a solution is:

```
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

反正排列组合的题都用backtracking

### Solution

代码如下
``` java
public class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        
        dfs(res, new ArrayList(), 1, n, k);
        
        return res;
    }
    
    public void dfs(List<List<Integer>> res, List<Integer> list, int start, int n, int k) {
        if( k == 0 ) {
            res.add(new ArrayList(list));
            return;
        } 
        
        for( int i = start; i <= n; i++ ){
            list.add(i);
            // 原来一只以为这里要新建一个的
            // 然而并不用
            dfs( res,list, i + 1, n, k-1 );
            // 因为递归完了，会一个个remove掉的
            list.remove(list.size()-1);
       } 
    }
}


```


*** 

Over！



















































