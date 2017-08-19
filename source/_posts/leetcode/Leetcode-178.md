---
title: Leetcode_178
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-06 22:00:00
---
Rank Scores

<!-- more -->

***
### Problem
给定 Score 表
```
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
```

排序得到
```
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+
```

### Solution

超时的方法，用 count 来计算 Rank
（可能因为每次都要distinct？
``` sql
SELECT
  Score,
  (SELECT count(distinct Score) FROM Scores WHERE Score >= s.Score) Rank
FROM Scores s
ORDER BY Score desc
```

先 DISTINCT 再 count，会快一点
这个只要distinct一次？
``` sql
SELECT
  Score,
  (SELECT count(*) FROM (Select DISTINCT Score from Scores) tmp 
  WHERE Score >= s.Score) Rank
FROM Scores s
ORDER BY Score desc
```
*** 

Over！










































