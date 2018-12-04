---
title: Mysql 联合索引
tags:
	- Mysql
categories:
	- Technology
	- Database
date: 2017-04-12 12:38:15
---
Mysql 联合索引

Reference: 
1. [MySQL索引原理及慢查询优化](http://tech.meituan.com/mysql-index.html "MySQL索引原理及慢查询优化")
2. [MySQL 联合索引详解](http://blog.csdn.net/lmh12506/article/details/8879916 "MySQL 联合索引详解")
3. [MySQL 复合索引+范围搜索中索引顺序的问题？](https://www.zhihu.com/question/31109426 "MySQL 复合索引+范围搜索中索引顺序的问题？")

<!-- more -->

***
联合索引又叫复合索引。

对于复合索引:
1. Mysql从左到右的使用索引中的字段，
2. 一个查询可以只使用索引中的一部份，但只能是最左侧部分。
3. 例如索引是key index (a,b,c). 可以支持 `a`，`a,b`，`a,b,c` 3种组合进行查找，但不支持 `b,c`进行查找。
    所以当最左侧字段是常量引用时，索引就十分有效。
    分别对A,B,C三个列建联合索引index，实际上是建立3个索引，每个索引都包含A:
    - `A,B,C`
    - `A,B`
    - `A`
4. 所以说创建复合索引时，应该仔细考虑列的顺序。

***

建索引的几大原则：
1. 左前缀匹配原则。一直向右匹配直到遇到 `(>、<、between、like)` 就停止
2. `=` 和 `in` 可以乱序
    a = 1 and b = 2 and c = 3 建立(a,b,c)索引可以任意顺序，mysql的查询优化器会帮你优化成索引可以识别的形式
3. 尽量选择区分度高的列作为索引
4. 索引列不能参与计算，保持列“干净”
5. 尽量的扩展索引，不要新建索引。

