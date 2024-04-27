---
title: "nginx防止域名被恶意指向"
description: "设置nginx只监听指定的域名请求，防止服务器IP被恶意解析。"
isCJKLanguage: false

lastmod: 2022-06-03T11:52:18+08:00
publishDate: 2022-06-03T11:52:18+08:00

categories:
 - nginx
tags:
 - nginx
 - 安全

toc: false
url: post/nginx-fang-zhi-yu-ming-bei-e-yi-zhi-xiang.html
---

设置nginx只监听指定的域名请求，防止服务器IP被恶意解析。

<!--more-->

nginx的默认配置为允许空主机头，用户可以通过IP来访问服务器资源，或者通过解析管理员未设置的域名来访问主机资源，会对对主机和IP的安全性造成威胁，如果被解析的IP未备案，可能导致管局封掉主机的IP地址，修改nginx默认配置来防止恶意解析。

# 配置
编辑conf/nginx.conf文件，修改server_name值为_，并设置返回的状态码为444
```
server {
listen 80 default;
server_name _;
return 444;
}
```
修改后需要重新载入配置文件才能生效

```
nginx -s reload
```
也可以设置服务器对恶意解析返回403状态码
```
server {
listen 80 default;
server_name _;
return 403;
}
```
