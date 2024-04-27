---
title: "NFS实现服务器间的目录共享"
description: "NFS（Network File System）即网络文件系统。多个客户端挂载同一服务端的目录资源，可实现目录资源的共享。"
keywords: "nfs"

date: 2022-09-11T10:16:02+08:00
lastmod: 2022-09-11T10:16:02+08:00

categories:
  - nginx
tags:
  - nginx


url: post/nfs-shi-xian-fu-wu-qi-jian-de-mu-lu-gong-xiang.html
math: mathjax
---

NFS（Network File System）即网络文件系统。多个客户端挂载同一服务端的目录资源，可实现目录资源的共享。

<!--more-->

# 安装部署
基础组件
服务器端和客户端均安装组件

```
yum -y install nfs-utils rpcbind
```
服务器端
创建共享目录

```
mkdir /data/nfs
```
NFS配置文件

```
cat vim /etc/exports
/data/nfs/ 10.10.10.0/24(rw,no_root_squash,no_all_squash,sync)
#/data/nfs/ 10.10.10.0/24(rw,no_root_squash,no_all_squash,async)
```
参数说明

- sync：数据同步写入磁盘
- async：数据可暂存于内存，可提高性能
重新加载配置文件

```
exportfs  -r
```
启动服务

```
service rpcbind start
```
查看状态（需要在hosts中做好本机的主机名与IP地址）

```
showmount -e 
```
客户端
创建挂载目录

```
mkdir -p /mnt/nfs
```
使用mount命令挂载

```
mount -t nfs 10.10.10.2:/data/nfs/ /mnt/nfs/
```
使用问题记录
服务端故障后，导致客户端系统卡住
当NFS服务端的故障，或者由于网络等原因，客户端无法连接到服务器端时，在客户端使用df命令，或者进入nfs目录，都会将当前终端卡住，并且服务器负载明显升高。此时无法通过umount卸载nfs目录解决问题，可以尝试修改/etc/mtab文件，删除nfs挂载相关的项目尝试解决

将moosefs目录作为nfs共享目录
将moosefs目录作为nfs共享目录时，使用exportfs -r查看，会显示
```
xportfs: /data/nfs requires fsid= for NFS export
```
需要在共享目录配置里加上 fsid=0参数
```
/data/nfs 192.168.0.0/24(rw,no_root_squash,no_all_squash,async,fsid=0)
```
