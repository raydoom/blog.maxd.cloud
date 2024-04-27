---
title: "避免mysql明文密码安全警告脚本"
description: "避免mysql在shell环境下直接执行命令时出现的安全警告."
keywords: "mysql,"

date: 2022-09-10T21:02:32+08:00
lastmod: 2022-09-10T21:02:32+08:00

categories:
  - mysql
tags:
  - 数据库
  - 安全

url: post/bi-mian-mysql-ming-wen-mi-ma-an-quan-jing-gao-jiao-ben.html
toc: true
---

避免mysql在shell环境下直接执行命令时出现的安全警告.

<!--more-->

当在shell终端中直接执行sql命令，并且用-p指定明文密码时，mysql会返回一条安全警告信息，此信息会包含在mysql返回的结果中，并且mysql并未提供隐藏此警告的选项，这为我们编写脚本，执行一些自动化任务带来不便，为了避免这种影响，可以先将查询结果写入到指定的临时文件，此时，警告信息被输出到了终端，临时文件中不包含警告信息，只包含查询结果，通过读取临时文件，获得正确的结果.

## mysql安全警告
```
mysql: [Warning] Using a password on the command line interface can be insecure.
```
脚本文件
```
#!/bin/bash
cmd="select * from xxx.xxx"
res=$(mysql -uroot -h192.168.0.90 -p111111 -s -e "${cmd}")
echo $res > /tmp/temp.txt

while read line
do
  res=$line
done < '/tmp/temp.txt'
rm -rf /tmp/temp.txt
#echo $res

exit;
```
其中res为最终结果

