---
title: "mysql事务隔离级别"
description: "mysql事务隔离级别设置."
keywords: "文章,目录,导航"

date: 2022-06-08T21:12:52+08:00
lastmod: 2022-06-08T21:12:52+08:00

categories:
 - 示例
tags:
  - 目录
  - 导航
  - 博客

toc: true
url: post/mysql-shi-wu-ge-li.html
---

mysql事务隔离级别设置.


概述
mysql事务隔离级别设置.

查询和设置
查看当前设置
查询全局和会话事务隔离级别
```
mysql> select @@tx_isolation;
mysql> select @@global.tx_isolation; 
mysql> select @@session.tx_isolation; 
```
设置隔离级别
设置当前会话隔离级别
```
mysql> set tx_isolation = 'read-committed';
```
设置系统隔离级别
```
mysql> set global tx_isolation = 'read-committed';
```
命令行操作，开始事务时
```
mysql> start transaction
```
配置文件写法
在my.cnf中加入一行
```
transaction-isolation = read-committed
```
事务隔离级别说明
事务隔离级别
```
read-uncommitted
```
可以看到未提交的数据（脏读）
```
read-committed
```
读取提交的数据。但是，可能多次读取的数据结果不一致（不可重复读，幻读）
```
repeatable-read (mysql默认隔离级别)
```
可以重复读取，但有幻读。读写观点：读取的数据行不可写，但是可以往表中新增数据。在mysql中，其他事务新增的数据，看不到，不会产生幻读。采用多版本并发控制（MVCC）机制解决幻读问题
```
serializable
```
可读，不可写。写数据必须等待另一个事务结束

