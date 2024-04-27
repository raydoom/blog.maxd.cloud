---
title: "mysql密码策略设置"
description: "mysql数据库的密码策略使用."
keywords: "mysql,密码"

date: 2022-06-04T19:46:45+08:00
lastmod: 2022-06-04T19:46:45+08:00

categories:
 - mysq
tags:
 - 安全
 - mysql
 - 数据库

url: post/mysql-mi-ma-ce-lue-shie-zhi.html
---

mysql数据库的密码策略使用.

mysql数据库通过插件validate_password实现密码策略功能，在5.6以及更早的版本，默认不使用密码策略，在5.7及更新的版本中，默认安装了密码策略插件并开启了密码策略.

安装插件
由于5.7及以上版本默认启用了validate_password插件，只有在5.6版本时才需要手动安装
```
mysql >INSTALL PLUGIN validate_password SONAME 'validate_password.so';
```
配置插件
修改插件配置
通过修改配置文件的[mysqld]项，来配置插件的使用参数
```
[mysqld]
plugin-load=validate_password.so
validate_password_policy=2
validate-password=FORCE_PLUS_PERMANENT
```
配置参数说明
```
validate-password=ON/OFF/FORCE/FORCE_PLUS_PERMANENT: 决定是否使用该插件(及强制/永久强制使用)。
validate_password_dictionary_file：插件用于验证密码强度的字典文件路径。
validate_password_length：密码最小长度。
validate_password_mixed_case_count：密码至少要包含的小写字母个数和大写字母个数。
validate_password_number_count：密码至少要包含的数字个数。
validate_password_special_char_count：密码至少要包含的特殊字符数。
validate_password_policy：密码强度检查等级，0/LOW、1/MEDIUM、2/STRONG。(0/LOW：只检查长度。1/MEDIUM：检查长度、数字、大小写、特殊字符。2/STRONG：检查长度、数字、大小写、特殊字符字典文件)
```
查看密码策略状态
```
mysql> show VARIABLES LIKE 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password_check_user_name    | OFF    |
| validate_password_dictionary_file    |        |
| validate_password_length             | 8      |
| validate_password_mixed_case_count   | 1      |
| validate_password_number_count       | 1      |
| validate_password_policy             | MEDIUM |
| validate_password_special_char_count | 1      |
+--------------------------------------+--------+
```
在线设置密码策略
```
mysql> set global validate_password_policy=0;
Query OK, 0 rows affected (0.00 sec)
mysql> set global validate_password_length=4;
Query OK, 0 rows affected (0.00 sec)
```