---
title: 《MySQL Crash Course》笔记
tags:
	- Mysql
	- Database
categories:
	- Techonology
	- Database
date: 2017-04-14 13:25:17
---
《MySQL Crash Course》

<!-- more -->

***

# 第一章 了解SQL

什么是数据库：
1. 区分 DB 和 DBMS （DB 是保存有组织的数据的容器）
2. 表 Table
3. 模式 schema ：关于数据库和表的布局以及特性
4. 列 column 
5. 数据类型 data type
6. 行：表中的一条记录
7. 主键：PK 能唯一区分表中的行 （唯一，非空）
8. SQL（Structured Query Language）

# 第二章 MySQL简介

MySQL：
1. `mysql -u root -p` 然后输入密码
2. 命令用 `;` 结尾
3. 用 `quit` 或者 `exit` 退出程序
4. MySQL Administrator：服务器管理
5. MySQL Query Browser：图形交互客户机，用来编写和执行 MySQL 命令

# 第三章 使用 MySQL 

使用 MySQL:
1. 选择数据库
		`USE <database name>;`
2. 列出所有数据库
		`SHOW DATABASES;`
3. 列出当前数据库中所有的表
		`SHOW TABLES;`
4. 显示当前表中所有的列
		`SHOW COLUMNS FROM <table name>` 或者 `DESCRIBE <table name>`
5. `auto_increment` 自动增量
		这样的话，必须记住最后一次使用的值
6. `HELP SHOW` 更多关于 `SHOW` 的语句

# 第四章 数据检索

检索数据：
1. 检索单个列：
		- `SELECT <COLUMN NAME> FROM <TABLE NAME>;`
		- 返回数据是未排序的，可能是被添加到表中的顺序，也可能不是。
		- 只要返回相同的行数，就是正常的
		- SQL语句不区分大小写
		- SQL语句不区分空格，所以可以分成多行写
2. 检索多个列：
``` sql
SELECT <COL1>, <COL2> 
FROM <TABLE NAME>;
```
3. 检索所有列：
``` sql
-- 使用通配符
SELECT *
FROM <TABLE NAME>;
-- 除非确实需要每一个列，通常不用通配符，否则会降低检索性能
```
4. 检索不同的行 `DISTINCT`
``` sql
-- 使用 DISTINCT 关键字
-- DISTINCT 关键字必须直接放在列的前面
SELECT DISTINCT vend_id
FROM products;
```
5. 限制结果数量 `LIMIT` 
``` sql
-- 使用 LIMIT 关键字
SELECT prod_name
FROM products
LIMIT 5;

-- 使用 LIMIT 关键字
-- 也可以指定开始行的位置
-- mysql 从第0行开始
SELECT prod_name
FROM products
LIMIT <start position> <row number>;

-- MySQL5 支持
SELECT prod_name
FROM products
LIMIT <row number> 
OFFSET <start position>;
```
6. 限定表的名字，和数据库的名字
``` sql
-- crashcourse 为数据库的名字
-- products 为表名字
SELECT products.prod_name
FROM crashcourse.products;
```

# 第五章 排序检索

排序数据：
1. 检索出的数据不是随机显示的，而是以底层表中出现的顺序显示
	- 可以是数据最初的添加顺序
	- 但是会随着更新删除，MySQL会重用回收存储空间，而改变顺序
	- 所以，如果不明确规定排序顺序，不应该假定检索出的数据又意义。
2. 子句 = 关键字 + 数据
	- 例如：Select 语句的 From 子句
3. Order By 子句
``` sql 
SELECT prod_name
FROM prodcts
ORDER BY prod_name

-- 实际上还可以通过非选择列排序
```
4. 按多个列排序
``` sql 
-- 首先按照价格排序，再按照名字排序
SELECT prod_id, prod_price, prod_name
FROM prodcts
ORDER BY prod_price, prod_name

-- 只有在price相同的时候，才会使用到按照name排序
```
5. 指定排序方向
	- `ASC` 其实没什么用，因为默认就是升序
	- `DESC` 降序
``` sql 
-- 按照价格 降序 排序
SELECT prod_id, prod_price, prod_name
FROM prodcts
ORDER BY prod_price DESC

-- 先按照价格 降序 排序，再按照名字默认的 升序 排序
SELECT prod_id, prod_price, prod_name
FROM prodcts
ORDER BY prod_price DESC, prod_name 

-- 如果想要多个列降序排序，必须在每一个列上都加上 DESC 关键字
```
6. 使用 `ORDER BY` 和 `LIMIT` 组合能找出最高的或者最低的
``` sql 
SELECT prod_price
FROM products
ORDER BY prod_price DESC
LIMIT 1;
```

# 第六章 过滤数据

过滤数据：
1. 使用 `WHERE` 子句
``` sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price = 2.50
ORDER BY prod_name;

-- ORDER BY 应该位于 where 子句之后
```
2. `WHERE` 子句操作符：
	- `=` 
	- `<>`, `!=` 不等于
	- `<`
	- `<=`
	- `>`
	- `>=`
	- `BETWEEN AND`
3. 范围值检查 `BETWEEN AND`：
``` sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price BETWEEN 5 AND 10;
```
4. 空值检查 `IS NULL` ：
	- 用 `IS NULL`
	- 在选择不具有特定值的时候，不返回 NULL，因为NULL具有特殊含义，数据库不知道他们是否匹配
``` sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price IS NULL;
```

# 第七章 高级数据过滤

高级数据过滤：
1. 组合 `WHERE` 子句
	- `AND`， `OR`
	- 用 `()` 来明确次序
2. `IN` 操作符
	- 合法值由逗号分隔后，全都列在圆括号中
	- `IN` 和 `OR` 完成的功能相同
	- `IN` 更清楚直观，执行更快
	- 可以包含别的 `SELECT` 语句
``` sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id IN (1002,1003)
ORDER BY prod_name;
```
3. `NOT` 操作符
	- `NOT` 否定后面的条件

# 第八章 用通配符进行过滤

通配符过滤：
1. `%` 通配符
	- 任何字符出现任意次数，包含0次
	- 尾空格会影响通配符匹配
	- 通配符不能匹配 `NULL`
``` sql
-- 查找 prod_name 以 ‘jet’ 为开头的产品
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE 'jet%';

-- 任何位置包含jet
WHERE prod_name LIKE '%jet%';

-- 指定开头结尾
WHERE prod_name LIKE 's%e';

```
2. `_` 下划线通配符，只能匹配单个字符（不能0个）
3. 但是是用通配符是有代价的，通常要花费更长的搜索时间

# 第九章 正则表达式

正则表达式：
1. 基本字符匹配
``` sql
-- 包含 '1000' 的prod_name
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000';

-- 或 关键字 |
WHERE prod_name REGEXP '1000|2000';

-- 匹配几个字符之一
WHERE prod_name REGEXP '[123] ton';

-- 匹配 除了括号中几个字符 以外的字符
WHERE prod_name REGEXP '[^123] ton';

-- 匹配 范围
WHERE prod_name REGEXP '[1-6] ton';

-- 匹配特殊字符 '.'，需要转义 （escaping）
-- 不转义的话，'.' 匹配任意字符
WHERE prod_name REGEXP '\\.';
```
2. 匹配字符类
3. 重复元字符
	- `*` 0或多
	- `?` 0或1，等于`{0,1}`
	- `+` 不少于1，等于`{1,}`
	- `{n}` 指定数目匹配
	- `{n,}` 不少于 n
	- `{n,m}` 匹配数目范围
4. 定位符
	- `^` 文本开始
	- `$` 文本结束
	- `[[:<:]]` 词的开始
	- `[[:>:]]` 词的结尾

# 第十章 创建计算字段

计算字段并不实际存在于数据库当中，而是运行时，在 Select 语句内创建
1. 拼接字段，利用 `Concat()` 函数
``` sql
SELECT Concat(vend_name, '(', vend_country, ')')
FROM vendors
ORDER BY vendor_name
```
2. 去掉头尾的空格 `TRIM()`
	- 去掉右边（尾部）的空格 `RTRIM()`
	- 去掉左边（尾部）的空格 `LTRIM()`
3. 使用别名 `AS` 
4. 执行算术计算
5. 测试计算
	可以通过省略 FROM 关键字进行测试。

# 第十一章 使用数据处理函数

文本处理函数：
1. `Upper()` / `Lower`
2. `Length()` 
3. `Locate()` 找出一个子串
4. `Left` / `Right`
5. `TRIM()` / `LTRIM()` / `RTRIM()`
6. `Soundex()` 

时间和日期处理：
1. 格式：yyyy-mm-dd

时间日期相关函数
1. AddDate()
2. AddTime()
3. CurDate()
4. CurTime()
5. Date()
	取出日期部分
``` sql
-- 找出九月一号的订单
SELECT cust_id, order_num
FROM orders
WHERE Date(order_date) = '2015-09-01';

-- 找出九月份的订单
SELECT cust_id, order_num
FROM orders
WHERE Date(order_date) BETWEEN '2015-09-01' AND '2015-09-30';
```
6. DateDiff()
7. Year() / Month() / Day() / Hour() / Minute() / Second()
``` sql
-- 找出九月份的订单
WHERE Year(order_date) =  2015 AND Month(order_date) = 09;
```

数值处理函数(略)

# 第十二章 汇总数据
汇总数据的例子：
1. 确定表中的行数
2. 获得表中行组的和
3. 找出表列的最大值，最小值，和平均值

MySQL的聚集函数：
1. AVG()
	- AVG()只能用来确定特定数值列的平均值，而且列名必须作为函数参数给出。
	- NULL值 AVG()函数忽略列值为NULL的行。
2. COUNT()
	- 使用COUNT(*)对表中行的数目进行计数，不管表列中包含的是空值（NULL）还是非空值。
	- 使用COUNT(column)对特定列中具有值的行进行计数，忽略NULL值。
	- 区别就在于有没有传参数
3. MAX()
4. MIN()
	- MySQL允许将它用来返回任意列中的最小值，包括返回文本列中的最小值。在用于文本数据时，如果数据按相应的列排序，则MIN()返回最前面的行。
5. SUM()

# 第十三章 分组数据
一个分组的例子
``` sql 
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id;

-- Vend_id    num_prods
-- 1001       3
-- 1002       2
-- 1003       7
-- 1005       3
```

`GROUP BY` 规定：
1. `GROUP BY` 子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
2. 如果在GROUP BY子句中嵌套了分组，数据将在最后规定的分组上进行汇总。
3. GROUP BY子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）
3. 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。
3. 如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
3. GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。

HAVING子句：
- HAVING非常类似于WHERE。
- 事实上，目前为止所学过的所有类型的WHERE子句都可以用HAVING来替代。
- 唯一的差别是WHERE过滤行，而HAVING过滤分组。

``` sql 
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id
HAVING COUNT(*)  >= 2;
```

HAVING和WHERE的差别,这里有另一种理解方法:
1. WHERE在数据分组前进行过滤，
2. HAVING在数据分组后进行过滤。
3. 这是一个重要的区别，WHERE排除的行不包括在分组中。这可能会改变计算值，从而影响HAVING子句中基于这些值过滤掉的分组。

# 第十四章 使用子查询
子查询最常见的使用是在WHERE子句的IN操作符中，以及用来填充计算列


# 第十五章 联结表
1. 等值联结
``` sql
SELECT vend_name, prod_name, prod_price 
FROM vendors, products 
where vendors.vend_id = products.vend_id;
```
2. 内部联结
``` sql
SELECT vend_name, prod_name, prod_price 
FROM vendors INNER JOIN products 
ON vendors.vend_id = products.vend_id;
```
3. 联结多个表
	- 联结多个表非常耗费资源

# 第十六章 高级联结

外部联结(LEFT JOIN / RIGHT JOIN)
- 外部联结还包括没有关联行的行
- 在使用OUTER JOIN语法时，必须使用RIGHT或LEFT关键字指定包括其所有行的表

# 第十七章 组合查询

MySQL允许执行多个查询（多条SELECT语句），并将结果作为单个查询结果集返回。这些组合查询通常称为并（union）或复合查询（compound query）。

TIPs：
1. 多数情况下，组合相同表的两个查询完成的工作与具有多个WHERE子句条件的单条查询完成的工作相同。
2. 换句话说，任何具有多个WHERE子句的SELECT语句都可以作为一个组合查询给出。
3. 这两种技术在不同的查询中性能也不同。因此，应该试一下这两种技术，以确定对特定的查询哪一种性能更好。

``` sql
-- 用 UNION
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION 
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id IN (1001,1002);

-- 用 WHERE OR
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5 
	OR vend_id IN (1001,1002)

-- 例子查询了同一张表，还可以 `UNION` 不同的表
```

`UNION` 原则：
1. 必须由两条 `SELECT` 语句组成，两两之间要用 `UNION` 连接
2. `UNION` 中的每个查询必须包含相同的列、表达式或聚集函数(次序可以不同)
3. 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐含地转换的类型（例如，不同的数值类型或不同的日期类型）。

重复行：
1. 如果有两条 SELECT 查到同一行， `UNION` 会自动去重。
2. 但是如果想要支持不去重，可以改用 `UNION ALL` 关键字。
3. `UNION ALL` 为 `UNION` 的一种形式，它完成 `WHERE` 子句完成不了的工作。

排序：
1. 在用 `UNION` 组合查询时，只能使用一条 `ORDER BY` 子句
2. 它必须出现在最后一条 `SELECT` 语句之后。
3. 然后对所有的结果集进行排序

# 第十八章 全文本搜索
两个最常使用的引擎为 `MyISAM` 和 `InnoDB` ，前者支持全文本搜索，而后者不支持。

通配符和正则表达式搜索的限制：
1. 性能
	会尝试匹配数据库中的所有表，所以会非常耗时
2. 明确控制
	很难（而且并不总是能）明确地控制匹配什么和不匹配什么。
3. 智能化的结果
	- 虽然基于通配符和正则表达式的搜索提供了非常灵活的搜索。
	- 但它们都不能提供一种智能化的选择结果的方法。
	- 啥意思呢？比如搜索包含一个单词的行。不能区分包含了单个匹配还是多个匹配

那怎么用全文本搜搜解决呢？
1. MySQL创建指定列中各词的一个索引，搜索可以针对这些词进行
2. 这样，MySQL可以快速有效地决定哪些词匹配（哪些行包含它们），
哪些词不匹配，它们匹配的频率，等等。

用法：
1. 为了进行全文本搜索，必须索引被搜索的列，而且要随着数据的改变不断地重新索引
2. 在索引之后，SELECT可与Match()和Against()一起使用以实际执行搜索。

启用：
1. 一般在创建表时启用全文本搜索。
2. CREATE TABLE语句接受FULLTEXT子句，它给出被索引列的一个逗号分隔的列表。
``` sql
CREATE TABLE productnotes
(
	note_id int           NOT NULL AUTO_INCREMENT,
	prod_id char(10)      NOT NULL,
	note_date datetime    NOT NULL,
	note_text text        NOT NULL,
	PRIMARY KEY(note_id),
	FULLTEXT(note_text) -- 对 note_text 这个列进行索引
) ENGINE = MyISAM;

-- 这样定义之后，数据库会在 note_text 增加，更新，删除行时，
-- 索引会随之自动更新
```

不要在导入数据时使用FULLTEXT：
更新索引要花时间，虽然不是很多，但毕竟要花时间。如果正在导入数据到一个新表，此时不应该启用 `FULLTEXT` 索引。应该首先导入所有数据，然后再修改表，定义 `FULLTEXT` 。这样有助于更快地导入数据（而且使索引数据的总时间小于在导入每行时分别进行索引所需的总时间）。

全文搜索：
1. 使用两个函数 `Match()` 和 `Against()` 执行全文本搜索
	- 其中 `Match()` 指定被搜索的列，这个(些)列必须在 `FULLTEXT()` 中定义过
	- `Against()` 指定要使用的搜索表达式。
``` sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('rabbit');
```
2. 全文本搜索的一个重要部分就是对结果排序。
	具有较高等级的行先返回（因为这些行很可能是你真正想要的行）。
	可以用以下的查询来查看，全文检索的 Rank：
	- 不包含 Rabbit rank 为 0
	- 包含 Rabbit 的行有不同的 rank
	- 等级由MySQL根据行中词的数目、唯一词的数目、整个索引中词的总数以及包含该词的行的数目计算出来。
``` sql
SELECT note_text, Match(note_text) Against('rabbit') AS rank
FROM productnotes;
```

查询拓展:
- 查询扩展用来设法放宽所返回的全文本搜索结果的范围
- 在使用查询扩展时，MySQL对数据和索引进行两遍扫描来完成搜索： 
	1. 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有行；
	2. 其次，查看上面行中的所有单词，选择出一些有用的词（拓展到要目标单词以外的单词）
	3. 最后，根绝这些拓展的词，再进行一遍全文搜索

布尔文本搜索：
- MySQL支持全文本搜索的另外一种形式，称为布尔方式（boolean mode）。
- 可以提供关于如下内容的细节：
	1. 要匹配的词；
	2. 要排斥的词；（如果某行包含这个词，则不返回该行，即使它包含其他指定的词也是如此）
	3. 排列提示（指定某些词比其他词更重要，更重要的词等级更高）；
	4. 表达式分组；
- 布尔方式不同于迄今为止使用的全文本搜索语法的地方在于， 即使没有定义 `FULLTEXT` 索引，也可以使用它。但这是一种非常缓慢的操作（其性能将随着数据量的增加而降低）。
``` sql 
-- 指定包含 heavy 且&& 不包含以rope开头的词
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against ('heavy -rope*' IN BOOLEAN MODE);
```
- 别的具体就省略不说了


注意点：
1. 索引全文本搜索时， MySQL 默认忽略短词（三个字母及以下）
2. MySQL 内建词库忽略
3. 词频很高忽略
4. 表中行数少于三行，不返回结果

# 第十九章 插入数据

不同的插入类型：
1. 插入完整的行：
``` sql
INSERT INTO Customers
VALUES (<所有字段的值>)
-- 主键自增的话，填 NULL
-- 这种语法很简单，但是不安全，应该避免使用

INSERT INTO Customers(cust_name,
	cust_mail)
VALUES (<对应字段的值>);
-- 可省略一些列，但必须是允许NULL的列，
-- 或者是，定义中给出默认值的列
```
2. 插入多个行
``` sql
INSERT INTO customers(cust_name,
	cust_mail)
VALUES (<对应字段的值>), (<第二组对应字段的值>);
```
3. 插入检索出的数据( `INSERT SELECT` )
```sql
INSERT INTO customers(cust_name,
	cust_email)
SELECT cust_name,
	cust_email
FROM cust_new;

-- SELECT 语句的列名其实不重要
-- 重要的是和 INSERT 对应的列的数据列型匹配，列的数量也要相同
-- SELECT 也能用 WHERE 进行过滤
```

# 第二十章 更新和删除

1. 更新：
``` sql
UPDATE customers
SET cust_email = 'eee@emai.com'
WHERE cust_id = 5;
-- 如果没有 where 会更新整张表的 email 字段
```
2. 删除：
``` sql 
-- 删除一行
DELETE FROM customers
WHERE cust_id = 10006;
```

注意：
1. 更新和删除的时候，除非确实对所有行进行操作，一定要带 WHERE
2. 先对 WHERE 进行 SELECT 测试

# 第二十一章 创建和操作表
1. 语法
2. 主键
3. NOT NULLL
4. AUTO_INCREMENT
5. 引擎类型
	- InnoDB是一个可靠的事务处理引擎，它不支持全文本搜索；
	- MEMORY在功能等同于MyISAM，但由于数据存储在内存（不是磁盘）中，速度很快（特别适合于临时表）；
	- MyISAM是一个性能极高的引擎，它支持全文本搜索，但不支持事务处理。
6. 更改表
	- ALTER TABLE







