---
title: CentOS-Nginx PHP-FPM 安装配置
author: 小敏-mingo
tags:
  - CentOS
  - CentOS 7
  - 服务器
  - 配置
  - Nginx
  - PHP-FPM
categories:
  - IT
  - 运维
abbrlink: fc47c0f9
date: 2019-02-19 21:54:11
---
Nginx（发音同engine x）是异步框架的 Web服务器，也可以用作反向代理，负载平衡器 和 HTTP缓存。
PHP-FPM(FastCGI Process Manager：FastCGI进程管理器)是一个PHPFastCGI管理器。用于替换 PHP FastCGI 的大部分附加功能，对于高负载网站是非常有用的。
<!-- more -->

## 安装 Nginx 服务
  
  - 安装 Nginx
  `yum install nginx`

  - 启动 Nginx，打开浏览器输入IP地址，出现 Nginx 欢迎页即成功
  `systemctl start nginx`

  - 设置 Nginx 服务开机启动
  `systemctl enable nginx`

  > 打开浏览器输入 IP 地址验证是否安装成功

## 安装 PHP-FPM 服务
  
  - 切换 PHP 源配置
  `yum-config-manager --enable remi-php72`

  - 安装 PHP-FPM 及相关扩展
  `yum install php php-fpm php-mbstring php-xml php-pdo php-mysqlnd php-devel php-bcmath php-gd php-zip`

  - 修改 php.ini 配置
  > 开启整站 zlib 压缩输出
  `zlib.output_compression = Off`
  > 修改 zlib 压缩比
  `zlib.output_compression_level = 5`
  > 关闭 PATH_INFO
  `cgi.fix_pathinfo = 0`
  > 设置时区
  `date.timezone = Asia/Shanghai`

  - 修改 www.conf 配置
  > 设置进程所属用户及用户组
  ```
  user = nginx
  group = nginx
  ```
  > 设置 unix socket 权限
  ```
  listen.owner = nginx
  listen.group = nginx
  listen.mode = 0660
  ```

  - 查看 PHP 版本
  `php --version`

  - 修改相关文件的所属
  ```
  chown root:nginx /var/lib/php/opcache
  chown root:nginx /var/lib/php/session
  chown root:nginx /var/lib/php/wsdlcache
  chown nginx:root /var/log/php-fpm
  ```
  
  - 启动 PHP-FPM 并设置服务开机启动
  ````
  systemctl start php-fpm
  systemctl enable php-fpm
  ```
