---
title: PHP-将Laravel部署到服务器
author: 小敏-mingo
tags:
  - PHP
  - PHP 7
  - Laravel
  - git
  - composer
categories:
  - IT
  - PHP
abbrlink: 1b7bf0e4
date: 2019-02-20 23:06:59
---
[Laravel](https://laravel.com/) 是一套简洁、优雅的PHP Web开发框架(PHP Web Framework)
<!-- more -->

## 前言
  > 完成 [CentOS-服务器初始化配置](http://blog.themismin.com/posts/fc47c0f9.html)
  > 申请 SSL 证书
  
## 部署
### git 安装
  `yum install git`

### composer 安装
  ```
  # 下载 composer.phar
  wget https://getcomposer.org/composer.phar
  #curl -sS https://getcomposer.org/installer | php

  # 移动 composer 命令至可执行程序目录
  mv composer.phar /usr/local/bin/composer

  # 修改 composer 命令权限
  chmod a+x /usr/local/bin/composer

  # 设置 composer 代理
  composer config -g repo.packagist composer https://packagist.phpcomposer.com

  # 清理 composer 缓存
  composer clearcache
  ```

### 部署站点
  ```
  # 添加站点用户
  useradd -s /sbin/nologin -g nginx mingo
  usermod -s /bin/bash mingo

  # 添加站点目录
  mkdir -p /var/www/mingo
  chown mingo:nginx /var/www/mingo
  ```

  > 参考 [CentOS-服务器初始化配置](http://blog.themismin.com/posts/fc47c0f9.html) 完成 mingo 用户的 ssh 配置
  ```
  su - mingo
  #...
  ```
  
  > 将 Jenkins 公钥添加至 mingo 公钥认证文件中

  > 添加当前服务器至 Jenkins 构建
  1. 打开 Jenkins 系统管理，在系统设置中添加 [Publish over SSH]

  2. 打开 Jenkins 相应的构建任务设置中添加 [构建后操作 - Send build artifacts over SSH]
  
  3. 点击构建任务完成构建

  > 添加 php-fpm 配置
  ```
  [mingo-api]
  user = mingo
  group = nginx
  listen = /var/run/mingo-api_php-fpm.sock
  listen.owner = mingo
  listen.group = nginx
  listen.mode = 0660
  listen.allowed_clients = 127.0.0.1
  pm = dynamic
  pm.max_children = 200
  pm.start_servers = 120
  pm.min_spare_servers = 100
  pm.max_spare_servers = 200
  slowlog = /var/log/php-fpm/mingo-api-slow.log
  php_admin_value[error_log] = /var/log/php-fpm/mingo-api-error.log
  php_admin_flag[log_errors] = on
  php_value[session.save_handler] = files
  php_value[session.save_path]    = /var/lib/php/session
  php_value[soap.wsdl_cache_dir]  = /var/lib/php/wsdlcache
  ```

  > 重启 php-fpm 服务
  `systemctl restart php-fpm`

  > 上传 SSL 证书，添加 nginx 配置
  ```
  server {
    listen       80;
    listen       [::]:80;
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/1_api.mingo.com_bundle.crt;
    ssl_certificate_key /etc/nginx/ssl/2_api.mingo.com.key;
    if ($scheme != "https") {
        return 301 https://$host$request_uri;
    }
    server_name  blog.mingo.com;
    root         /var/www/mingo/public;
    access_log  /var/log/nginx/mingo-api_access.log  main;
    error_log   /var/log/nginx/mingo-api_error.log;
    index index.php index.html index.htm;
    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }
    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/mingo-api_php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
    location ~ /\.ht|/\.git {
        deny  all;
    }
  }
  ```

  > 重启 nginx 服务
  `systemctl restart nginx`

  * PS: 以上配置基于单个请求 PHP-FPM 占用 30M/进程，nginx 占用内存相对于 PHP-FPM 忽略不计，预留 1G 内存计算。 7G * 1024M / 30M = 238 月等于 200 个进程
