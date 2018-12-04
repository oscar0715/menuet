---
title: Leetcode_175
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-06 21:49:00
---
Database Combine Two Tables

<!-- more -->

***

### Problem
区分 
1. inner join （on 的属性 两张表都要有）
2. left join （有左表所有的记录
3. right join （有右表所有的记录

### Solution

``` sql
Select Person.FirstName,Person.LastName, Address.City, Address.State 
from Person 
left JOIN Address on Address.PersonId = Person.PersonId;

```


*** 

Over！










































