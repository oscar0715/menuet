---
title: Mysql Index
tags:
	- Mysql
categories:
	- Technology
	- Database
date: 2017-08-07 09:48:00
---
Mysql 索引 INDEX

引用：
[MYSQL-索引](https://segmentfault.com/a/1190000003072424)
[美团-MySQL索引原理及慢查询优化](https://tech.meituan.com/mysql-index.html)
[MySQL Explain详解](http://www.cnblogs.com/xuanzhi201111/p/4175635.html)
[MySQL索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)
[Mysql Explain 详解](http://www.cnitblog.com/aliyiyi08/archive/2008/09/09/48878.html)_
<!-- more -->

***

# B-Tree 索引

## 索引类型
1. 普通索引，允许重复
    - `CREATE INDEX 索引名 ON 表名 (列名1, 列名2);`
    - `ALTER TABLE 表名 (列名1, 列名2);``
    - 创建表时，指定索引
2. Unique 索引
    - `CREATE UNIQUE INDEX`
3. 主键索引：最快的索引

## 索引语法
1. 删除
`DROP INDEX index_name ON table_name;` 
`ALTER TABLE table_name DROP INDEX index_name;`
`ALTER TABLE table_name DROP PRIMARY KEY;`
2. 查看
`show INDEX from tblname;`

## 索引的选择
1. 选择性 (selectivity) = 不重复的索引值 / 表的记录数
    - （不重复的索引值，也叫做基数 Cardinality）
    - 选择性越大越好

<br>
***
<br>

# 索引原理

## 磁盘IO与预读
**磁盘读取**
磁盘读取靠的是机械运动，读取数据花费的时间可以分为三个部分：
1. 寻道时间：5ms左右
2. 旋转延迟，以7200转的磁盘为例：
    - 磁盘转速：7200次/min = 120次/s 
    - 旋转延迟 = 1/120/2 = 4.17ms
3. 传输时间：从磁盘或者写入磁盘的时间，0.几毫秒，所以相对可以忽略

**预读**
所以，一次磁盘 IO 的时间一共为 9ms 左右。作为比较，一次IO的时间可以执行40万条指令，所以IO的代价是很高的。所以操作系统做了一些优化，不光把当前磁盘的地址的数据督导内存缓存区内，还把相邻地址的数据也读到了内存缓存区内，所以相邻地址的数据的访问会很快。
1. 每次IO读取的数据，我们称之为一页 Page，一页的大小和操作系统有关，一般为4k 或者 8k

## 数据结构
**B+树**
![B+树](https://tech.meituan.com/img/mysql_index/btree.jpg)

1. 非叶子节点不存储真实的数据，只存储搜索方向的数据项
2. 数的高度h决定发生IO的次数
    - 数据总量是 `N`，每个磁盘块可以放的数据的条数为 `m`
    - 那么，高度的计算公式为 $h = \log_{m+1} N$
    - `m = 磁盘块的大小 / 数据项的大小` 一般来说磁盘块的大小是个固定的，那么数据项越小， `m` 越大，那么 `h` 可以更小，所以数据项越小，查询可以更快。
    - 所以索引字段要尽量小。这也是把数据放到叶子结点的原因，否则 `m` 会大很多
3. InnoDB 叶子节点直接存的数据记录的主键，MyISAM 叶子节点保存数据记录的地址（引用）

<br>
***
<br>

# 联合索引
范围列可以用到索引（必须是最左前缀），但是范围列后面的列无法用到索引。同时，索引最多用于一个范围列，因此如果查询条件中有两个范围列则无法全用到索引。

<br>
***
<br>

# Explain
MySQL [communitycenter]> explain select * from index_white_user;
```
+----+-------------+------------------+------+---------------+------+---------+------+------+-------+
| id | select_type | table            | type | possible_keys | key  | key_len | ref  | rows | Extra |
+----+-------------+------------------+------+---------------+------+---------+------+------+-------+
|  1 | SIMPLE      | index_white_user | ALL  | NULL          | NULL | NULL    | NULL |   30 | NULL  |
+----+-------------+------------------+------+---------------+------+---------+------+------+-------+
```

**type**
表示MySQL在表中找到所需行的方式，又称“访问类型”。
1. 常用的类型有： ALL, index,  range, ref, eq_ref, const, system, NULL（从左到右，性能从差到好）
    - ALL：Full Table Scan， MySQL将遍历全表以找到匹配的行
    - index: Full Index Scan，index与ALL区别为index类型只遍历索引树
    - range:只检索给定范围的行，使用一个索引来选择行（IN 也会用range表示）
    - ref: 表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值
    - eq_ref: 类似ref，区别就在使用的索引是唯一索引，对于每个索引键值，表中只有一条记录匹配，简单来说，就是多表连接中使用primary key或者 unique key作为关联条件
    - const、system: 当MySQL对查询某部分进行优化，并转换为一个常量时，使用这些类型访问。如将主键置于where列表中，MySQL就能将该查询转换为一个常量,system是const类型的特例，当查询的表只有一行的情况下，使用system。
    表最多有一个匹配行，它将在查询开始时被读取。因为仅有一行，在这行的列值可被优化器剩余部分认为是常数。const表很快，因为它们只读取一次！
    表示主键索引或者唯一索引
    - NULL: MySQL在优化过程中分解语句，执行时甚至不用访问表或索引，例如从一个索引列里选取最小值可以通过单独索引查找完成。

**key_len**
表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度（key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的）
不损失精确性的情况下，长度越短越好 





