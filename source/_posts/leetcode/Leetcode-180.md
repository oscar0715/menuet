---
title: Leetcode_180
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-06 09:30:00
---
Consecutive Numbers

<!-- more -->

***

### Problem

找出在 Num 栏连续出现的三个数

```
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```

### Solution

超时的方法
``` sql
Select DISTINCT l1.Num as ConsecutiveNums 
from Logs as l1, Logs as l2, Logs as l3
Where l2.Id = l1.Id + 1 and l3.Id = l2.Id + 1 and l1.Num = l2.Num and l1.Num = l3.Num

```


*** 

Over！










































