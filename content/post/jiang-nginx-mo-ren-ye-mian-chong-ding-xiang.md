---
title: "将nginx默认页面重定向"
description: "将nginx的默认欢迎页面重定向到特定的域名。"

lastmod: 2022-06-03T16:43:23+08:00
date: 2022-06-02T11:52:03+08:00

categories:
 - nginx
tags:
 - nginx

url: post/jiang-nginx-mo-ren-ye-mian-chong-ding-xiang.html
toc: true
---

将nginx的默认欢迎页面重定向到特定的域名。

<!--more-->
# 配置
修改nginx目录下html目录里面的index.html文件

原文件
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
body {
width: 35em;
margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif;
}
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
修改后
```
<!DOCTYPE html>
<html>
<head>
<title>hello</title>
<style>
body {
width: 35em;
margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif;
}
</style>
</head>
<body>
<h1>nihao</h1>
<script>
window.location.href = "https://raydoom.github.io/";
</script>
</body>
</html>
```
其中https://raydoom.github.io/为重定向的网址