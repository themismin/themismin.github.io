---
title: 运维-python开发加密安装
tags:
  - python
categories:
  - IT
  - 运维
abbrlink: 585b7ff6
date: 2020-05-14 16:12:36
---

## 安装配置
```
yum -y install wget vim gcc yum-utils git net-tools lrzsz libsodium

yum install m2crypto python-setuptools && easy_install pip

git clone -b manyuser https://github.com/shadowsocksr-backup/shadowsocksr.git

pip install shadowsocks
```