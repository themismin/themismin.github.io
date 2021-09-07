---
title: 运维-centos 搭建BT下载服务器
author: 小敏-mingo
tags:
  - Transmission
  - BT
categories:
  - IT
  - 运维
abbrlink: f6e9cc7b
date: 2019-06-20 11:36:27
---
Transmission是一种BitTorrent客户端，特点是一个跨平台的后端和其上的简洁的用户界面。
<!-- more -->

## 安装 & 启动
```
yum install epel-release
yum install transmission-daemon

systemctl start transmission-daemon
systemctl enable transmission-daemon
```

## 配置
> 配置文件 /var/lib/transmission/.config/transmission-daemon/settings.json
```
"rpc-password": "mingo", #web-ui的密码，可直接修改，重新运行或者reload服务的时候会自动被加密
"rpc-username": "mingo", #远程电脑登入web-ui的用户名称
"rpc-whitelist": "*",  #允许远程连接控制的电脑IP地址白名单，如果只允许局域网内电脑控制的话可以用“192.168.*.* ”
"rpc-whitelist-enabled": true, #如果你要让其他网段连入，请设false
```