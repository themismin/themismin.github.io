---
title: PHP-异步网络通信引擎swoole安装
author: 小敏-mingo
tags:
  - PHP
  - PHP 7
  - swoole
categories:
  - IT
  - PHP
abbrlink: 8fb54a41
date: 2019-06-24 14:02:01
---
Swoole：面向生产环境的 PHP 异步网络通信引擎
<!-- more -->

## 前言
基于 PHP 7.0.0 或以上版本 (版本越高性能越好)

## 安装
### 自动安装
```
pecl install swoole
```

### 手动编译
```
git clone https://github.com/swoole/swoole-src.git
cd swoole-src
phpize
./configure --enable-openssl --enable-sockets --enable-http2 --enable-mysqlnd
make
make install
```

## 异常
- 升级GCC
```
yum install centos-release-scl
yum install devtoolset-7
scl enable devtoolset-7 bash
```

- 安装pecl
```
yum install php-pear php-devel
```