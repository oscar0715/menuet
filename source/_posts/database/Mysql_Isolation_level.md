---
title: Mysql 事务隔离级别
tags:
	- Mysql
categories:
	- Technology
	- Database
date: 2017-04-11 22:07:15
---
Mysql 事务隔离级别
Reference: [Mysql 事务隔离级别视频](http://www.jikexueyuan.com/course/1524.html "Mysql 事务隔离级别视频")
<!-- more -->

***

# MySQL 事务隔离级别的概念
1. Serialize 序列化 
	`start transaction` 以后，必须 `commit`; 然后另外一个 `session` 才能进行操作，否则会被 `block` 住
2. Repeatable Read 可重复读 （mysql 默认级别）
	sessionA 开始了 transaction 以后，所有的读到的结果都是一致的。哪怕另外一个 sessionB 在中间修改了，还是会读到修改之前的结果。
	但是比如，如果 sessionA 修改到了 sessionB 插入的一行，那么 sessionB 之前修改的结果现在也能看到。
	这就出现 `幻读`
3. Read Committed  提交可读
	顾名思义，只要 committed 都能看到。于是多次读的结果也可能不一样了。
4. Read Uncommitted 未提交可读
	顾名思义，未提交的都能读到。如果最后另一个 Session 的修改没有成功，在当前session可能出现 `脏读`

# MySQL 性能
还是那句话，`Everything has a price to pay`。
比如 innodb 支持事务处理，但是因此性能会差一下。

如何提高性能建议：
1. 使用小事务
2. 选择合适的隔离级别
3. 避免死锁
4. 保证事务开始之前，一切都是可行的，否则事务就block住了








