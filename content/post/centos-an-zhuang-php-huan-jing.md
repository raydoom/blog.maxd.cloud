---
title: "CentOS安装php环境"
description: "使用yum方式在线安装php环境。"
keywords: "centos,php"

date: 2022-06-01T15:59:41+08:00
lastmod: 2022-06-01T15:59:41+08:00

categories:
 - php
tags:
 - centos
 - php

url: post/centos-an-zhuang-php-huan-jing.html
toc: true
---

使用yum方式在线安装php环境.

<!--more-->

# 安装
添加webtatic yum源

## webtatic EL7 for CentOS/RHEL 7
```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```
## webtatic EL6 for CentOS/RHEL 6
```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm
```
安装php环境及相关组件
```
yum install php55w php55w-bcmath php55w-cli php55w-common php55w-devel php55w-fpm php55w-gd php55w-imap php55w-ldap php55w-mbstring php55w-odbc php55w-pdo php55w-pear php55w-pecl-igbinary php55w-xml php55w-xmlrpc php55w-opcache php55w-intl php55w-pecl-memcache php55w-pecl-redis php55w-mysql
```
> 如需安装其他版本，只需替换版本号，例如，需要安装5.6版，则将55改为56即可

查看php版本
```
$ php -v
PHP 5.5.9 (cli) (built: Feb 11 2014 08:25:33)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
```
在apache目录/var/www/html/下面新建文件phpinfo.php
```
$ cat /var/www/html/phpinfo.php
<?php      
phpinfo();
?>
```
启动apache

```
service httpd start
```
访问http://IP/phpinfo.php

php55w安装mongodb扩展
对于php55w及以下的版本，webtatic源并没有mongodb的扩展，需要通过pecl安装

```
pecl install mongodb
```
安装完成后，在php.ini配置扩展

```
echo extension=mongodb.so >> /etc/php.ini
```
重启apache
```
service httpd restart
```
访问phpinfo.php查看扩展详情

对于php56w及以上的版本，可以直接使用yum安装

```
yum install php56w-pecl-mongodb
```