---
title: HBase Introduction
tags:
  - HBase
categories:
  - Technology
  - Database
date: 2017-04-08 22:04:15
---
HBase Introduction
<!-- more -->

***

# Hbase
Hbase 是开源版本的 Google BigTable 分布式存储系统。 BigTable 是一个分布式，可拓展，高性能的数据库。

HBase是运行在 Hadoop Distributed File System （HDFS）上的。HDFS 则是一个分布式文件系统，它把文件分割为 block， 并存储了几个副本在不同的机器上。 HDFS 为 HBase 提供了一个可靠地可拓展的文件系统。

# HBase 的 Data Modal
![Table in HBase](https://oli.cmu.edu/repository/webcontent/4b0ed9a580020ca601a544e41277d117/_u04_cloud_storage/_u04_m03_NoSQL_databases_case_studies/webcontent/HBASE_table.png "Table in HBase")

HBase 中一行代表一条记录，每一行用一个 `row key` 来作为主键。 与关系型数据库不同的是，HBase中的主键是一个字节数组。所以理论上，从字符串，整数，double，long等各种数据类型被转换为字节数组以后都可以作为 `row key`。HBase在存储数据的时候，自动回根据 `row key` 把数据排序。

HBase 中的每一个 `column` 有自己的 `column name`。 而且几个 `column` 可以进一步组合为 `column family`。 同属于一个 `column family` 的 `column` 有一个共同的前缀。

例如：`Metadata:Type` 和 `Metadata:Language` 同属于 `Metadata` `column family`

在 HBase column 的数据类型不用定义，因为也是存储为字节数组的，这样 HBase 也可以存储任何格式的数据，然不会 validate 不通过。

在定义表的时候，`column family` 必须实现定义好，不过 `column` 可以后来按需求增加。HBase把所有的 `column family` 成员存储在一起，所有被称为 `columnar database`，列式数据库

HBase  对每个 cell 提供了时间戳，来维护不同的 version。`{row,column,version}` 可以在 HBase 表中找到一个特定的值。

排序顺序:
1. `row key` 
2. `column family`
3. `family members`
4. `timestamp`

# HBase 的操作
HBase 有四种操作： `Get`, `Put`, `Scan`, 和 `Delete`.
1. `Get`: 返回一行的所有 cell （可以理解为返回一条记录）
2. `Put`: 可以用来添加或者更新
3. `Scan`: 可以可以返回多行操作
4. `Delete`: 删除一行

注意：
1. `Get`，`Scan`，`Delete` 都对最新的 `version` 进行操作
2. `Put` 则创建一个新的版本存入数据库中
3. 当然删除也可以针对某一个 `version` 进行

# HBase Architecture
HBase 是由一组 `HBase node` 组成的
1. 一台为 `master`
2. 其他为 `slave`

HBase 使用 Apache Zookeeper 作为 cluster 的分布调度服务。
1. 它负责选择 master 节点
2. 节点注册

![HBase architecture](https://oli.cmu.edu/repository/webcontent/4b0ed9a580020ca601a544e41277d117/_u04_cloud_storage/_u04_m03_NoSQL_databases_case_studies/webcontent/HBASE_archi.png "HBase architecture")

# Data Partition
为了实现 HBase 的可拓展性，当数据量很大的时候，需要将数据分布到不同的节点上。

HBase 会自动将一张表分割成几个区域，每个区域都有一段连续的 row。

![Splitting a table into multiple regions in HBase](https://oli.cmu.edu/repository/webcontent/4b0ed9a580020ca601a544e41277d117/_u04_cloud_storage/_u04_m03_NoSQL_databases_case_studies/webcontent/HBASE_regions.png "Splitting a table into multiple regions in HBase")

在表小的时候整张表都在一个 region 当中，随着表越来越大，HBase会对此进行自动分割。

# Client Access
用户是怎么在 cluster 中找到要操作的数据的呢？

为了保存所有的 region 信息和他们的位置， HBase 维护了两张目录表，这两张目录表也其他表一样存在 HBase 中，这样可以实现 fault tolerance
1. `-ROOT-`
    `-ROOT-` 会一直保存在一个 region 中，
2. `.META.`
    `.META.` 可能会被分割到不同 region 中

用户先连接到 `cluster，` 通过 `ZooKeeper` 找到 `-ROOT-` 表，然后通过 `-ROOT-` 表找到 `.META.` 表的位置。然后通过 `.META.` 表找到用户要找的 `row` 所在的 `regionserver` 。用户找到了 `regionserver` 以后就可以直接对 `regionserver` 进行操作了。

# Write Operation
一个 RegionServer 在执行写操作的时候要经过一下步骤
1. 把写操作添加到 commit log 中 （commit log 在 HDFS）
2. 然后写操作会被添加到内存里 （in-memory cache）
3. 在内存满的时候，这些内容才会被添加到文件系统上

因为 commit log 是存在 HDFS ，所以当一台 regionServer 挂掉以后，我们还能从别的 node 上得到副本，然后根据commit log 来恢复这台 regionServer 在挂前的状态。

# Read Operation
根绝前面的写操作，读操作相应有以下顺序：
1. 先访问内存（in-memory cache）
2. 如果内存上没有，再去访问文件系统上的 flushed file ，访问顺序是从最新的文件到最旧的文件。
3. 随着 flushed file 的增加，HBase 会自动 compact flushed files，如果某条记录的 version 超过了一定的数量就会被删除。

# ACID Properties in HBase
ACID：
1. Atomicity: 
    HBase 的更新操作具有原子性；
2. Consistency: 
    对于单条 row 来说呢，Get 操作具有一致性。
    但是对于 scan 来说就不一定了。
    但是至少一条 row 是具有原子性的
3. Isolation: 
    对于一条 row 的操作都是 Isolation的
    但是 scan 同样不一定
4. Durability:
    具有 durability













