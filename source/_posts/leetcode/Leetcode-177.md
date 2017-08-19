---
title: Leetcode_177
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-06 21:51:00
---
Nth Highest Salary

<!-- more -->

***

### Solution



用 Distinct，limit，offset
``` sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
DECLARE M INT;
SET M=N-1;
  RETURN (
      select (
          select distinct Salary from Employee order by Salary Desc limit 1 offset M
        ) 
  );
END
```

*** 

Over！










































