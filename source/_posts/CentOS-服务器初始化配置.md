---
title: CentOS-服务器初始化配置
author: 小敏-mingo
tags:
  - CentOS
  - CentOS 7
  - 服务器
  - 配置
  - yum
  - sshd
categories:
  - IT
  - 运维
abbrlink: 9d04baee
date: 2019-02-18 18:43:52
---
CentOS（Community Enterprise Operating System，中文意思是社区企业操作系统）是Linux发行版之一，它是来自于Red Hat Enterprise Linux依照开放源代码规定释出的源代码所编译而成。由于出自同样的源代码，因此有些要求高度稳定性的服务器以CentOS替代商业版的Red Hat Enterprise Linux使用。两者的不同，在于CentOS完全开源。
<!-- more -->

## 前言
  > 以下配置命令基于 CentOS 7 操作系统

## 更新 & 安装基础软件
  > 直接执行 yum -y update

  > 安装基础软件 yum -y install wget vim gcc yum-utils

## 配置 yum 源
  
```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-7.rpm
yum makecache
```

## 修改主机名
  > 执行 hostname 查看主机名
  > 执行 hostname mingo 设置主机名
  > 编辑 /etc/hostname 修改主机名
  > 编辑 /etc/sysconfig/network 修改网络配置信息中的主机名
  > 编辑 /etc/hosts 修改映射的主机名

## 生成 SSH 公钥
  > 执行 ssh-keygen

## 配置 SSH 免密码登录
  > 编辑 ~/.ssh/authorized_keys 添加公钥
  > 变更文件权限 chmod 600 ~/.ssh/authorized_keys
  > 编辑 /etc/ssh/sshd_config 修改配置，禁止密码认证 PasswordAuthentication no
  > 执行 systemctl restart sshd 命令，重启 ssh 服务 

## 重启服务器
  > 执行 reboot 命令
