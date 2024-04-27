---
title: "Inception部署配置"
description: "一个集审核、执行、回滚于一体的一个自动化运维工具，它是根据MySQL代码修改过来的，用它可以很明确的，详细的，准确的审核MySQL的SQL语句."
keywords: "mysql,inception"

date: 2022-09-18T20:58:13+08:00
lastmod: 2022-09-18T20:58:13+08:00

categories:
  - mysql
  - inception
tags:
  - mysql
  - inception

url: post/mermaid-charts.html
toc: true
---

一个集审核、执行、回滚于一体的一个自动化运维工具，它是根据MySQL代码修改过来的，用它可以很明确的，详细的，准确的审核MySQL的SQL语句.

<!--more-->

Inception是集审核、执行、回滚于一体的一个自动化运维工具，它是根据MySQL代码修改过来的，用它可以很明确的，详细的，准确的审核MySQL的SQL语句，
它的工作模式和MySQL完全相同，可以直接使用MySQL客户端来连接，但不需要验证权限，它相对应用程序（上层审核流程系统等）而言，是一个服务器，
在连接时需要指定服务器地址及Inception服务器的端口即可，而它相对要审核或执行的语句所对应的线上MySQL服务器来说，是一个客户端，
它在内部需要实时的连接数据库服务器来获取所需要的信息，或者直接在在线上执行相应的语句及获取binlog等，Inception就是一个中间性质的服务。


# Inception的架构

Yearning SQL 审计平台 基于Vue.js与Django的整套mysql-sql审核平台解决方案。
提供基于Inception的SQL检测及执行。
Yearning 是基于Inception的web可视化SQL审核平台,其本身只提供可视化交互页面并不具备sql审核的能力。所以必须搭配Inception一起使用。

# 部署步骤
创建备份/回滚服务器
Inception在做DML操作时，具有备份功能，它会将所有当前语句修改的行备份下来，存储到一个指定的库中，创建一个mysql数据库实例作为Inception的备份/回滚服务器

目标数据库配置
Inception所操作的目标数据库实例需开启bin-log日志，并且格式为RAW，才能使用备份/回滚功能，连接信息将在Yearning的WEB管理界面添加，数据库配置文件配置项目
```
log-bin = mysql-bin
binlog-format = raw
binlog_row_image = full
server_id = 12
配置Inception
inception的默认配置文件为/etc/inc.cnf

[inception]
general_log=1
general_log_file=inception.log
port=6669
socket=/tmp/inc.socket
character-set-client-handshake=0
character-set-server=utf8
inception_remote_system_password=111111
inception_remote_system_user=root
inception_remote_backup_port=3311
inception_remote_backup_host=192.168.0.64
inception_support_charset=utf8,utf8mb4
inception_osc_on=ON
inception_osc_bin_dir=/usr/bin
```
配置项说明
```
inception_remote_backup_host //远程备份库的host
inception_remote_backup_port //远程备份库的port
inception_remote_system_user //远程备份库的用户
inception_remote_system_password //远程备份库的用户密码
```
启动Inception
配置完成后，使用此配置文件启动Inception，使用容器方式部署

```
docker run -d --name inception -v /local_path/inc.cnf:/etc/inc.cnf -p 6669:6669 -d registry.cn-hangzhou.aliyuncs.com/lihuanhuan/inception
```
也可以不使用-v卷映射方式，在启动容器后，进入容器修改配置文件，重启容器生效

查看Inception状态
Inception使用mysql客户端进行连接

```
mysql -uroot -h192.168.0.64 -P6669
mysql >inception get variables;
```
配置Yearning
Yearning基于Django框架开发，后端需要使用mysql数据库
创建数据库，下载并导入初始化语句

```
wget https://raw.githubusercontent.com/cookieY/Yearning/master/install/yearning-docker-compose/init-sql/install.sql
mysql >CREATE DATABASE Yearning DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
mysql >use Yearning; 
mysql >source install.sql;
```
启动Yearning
使用容器方式部署Yearning，并连接到数据库

```
docker run -d -e HOST=192.168.0.64 -e MYSQL_ADDR=192.168.0.64 -e MYSQL_USER=root -e MYSQL_PASSWORD=111111 -p8080:80 -p8000:8000 registry.cn-hangzhou.aliyuncs.com/cookie/yearning:v1.3.0
```
访问主机的8080端口，登入Yearning，默认用户名密码为 admin/Yearning_admin

进入管理->设置，配置Inception信息和备份库的地址信息，备份库的地址信息需要与Inception配置文件中的保持一致，经测试，此处不配置备份库的信息也能实现回滚功能，使用的备份库实例为Inception配置文件中所配置的信息。


