---
title: 运维-代理服务器squid安装
author: 小敏-mingo
tags:
  - 代理服务器
  - squid
categories:
  - IT
  - 运维
abbrlink: 8ce0eb51
date: 2021-06-02 10:18:27
---
Squid Cache（简称为Squid）是HTTP代理服务器软件。Squid用途广泛，可以作为缓存服务器，可以过滤流量帮助网络安全，也可以作为代理服务器链中的一环，向上级代理转发数据或直接连接互联网。Squid程序在Unix一类系统运行。由于它是开源软件，有网站修改Squid的源代码，编译为原生Windows版[2]；用户也可在Windows里安装Cygwin，然后在Cygwin里编译Squid。
<!-- more -->

## 安装
```
yum install squid
yum start squid
yum enable squid
```

## 配置
vim /etc/squid/squid.conf
```
...#14 配置允许访问的IP地址
acl allowip src 120.26.123.232
acl allowip src 120.26.209.207
...

...#57
http_access allow allowip
...

...
# Leave coredumps in the first cache dir
coredump_dir /var/spool/squid

via off
forwarded_for delete
...

```