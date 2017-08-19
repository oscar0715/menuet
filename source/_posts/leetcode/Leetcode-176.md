---
title: Leetcode_176
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-06 21:50:00
---
Second Highest Salary

<!-- more -->

***

### Solution

可以用 max 解决
``` sql
SELECT max(Salary) as SecondHighestSalary
FROM Employee
WHERE Salary < (SELECT max(Salary) FROM Employee)

```

也可以用 Distinct，limit，offset
``` sql
select (
  select distinct Salary from Employee order by Salary Desc limit 1 offset 1
) as SecondHighestSalary
```

*** 

Over！










































