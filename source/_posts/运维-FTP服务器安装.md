---
title: 运维-FTP服务器安装
author: 小敏-mingo
tags:
  - FTP
  - 服务器
  - FlashFXP
categories:
  - IT
  - 运维
abbrlink: 828d97a9
date: 2019-01-28 22:35:12
---
FTP服务器（File Transfer Protocol Server）是在互联网上提供文件存储和访问服务的计算机，它们依照FTP协议提供服务。 FTP是File Transfer Protocol(文件传输协议。顾名思义，就是专门用来传输文件的协议。简单地说，支持FTP协议的服务器就是FTP服务器。
<!-- more -->

## 安装
  > 直接执行 yum install vsftpd

  > 设置开机启动

```
systemctl start vsftpd
systemctl enable vsftpd
```

## 配置
  > 修改配置文件
  > vim /etc/vsftpd/vsftpd.conf

```
anonymous_enable=NO

local_enable=YES

write_enable=YES

local_umask=002

local_root=/data/part1/ftp
```

  > 添加用户组及用户
  > useradd -m -d /home/ftpuser -s /sbin/nologin ftpuser
  > passwd ftpuser

  > useradd -m -d /home/ftpxumin -s /sbin/nologin -g ftpuser ftpxumin
  > passwd ftpxumin

## 其它
### Windows FlashFXP 乱码问题
> 字符编码设置成 UTF-8