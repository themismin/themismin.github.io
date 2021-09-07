---
title: 运维-Laravel 开发文档
abbrlink: 6141b706
date: 2020-12-21 11:24:10
tags:
---
Laravel 是一个由Taylor Otwell所创建，免费的开源[3] PHP Web 框架，旨在实现的Web软件的MVC架构，并作为CodeIgniter的替代方案。
<!-- more -->

### 创建项目
```bash
composer create-project --prefer-dist laravel/laravel htdoc
```

### coding配置
```bash
git init
git remote add origin git@domian:user/project-name.git
git fetch
git pull origin master

git config user.name "user"
git config user.email "user@domain.com"
```

### composer配置
```bash
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
### 申请StartSSL证书
1. 生成CSR（Certificate Signing Request）证书注册请求。
```bash
openssl req -nodes -newkey rsa:2048 -keyout com.domain.key -out com.domain.csr
```

### IDE Helper 配置
```bash
composer require barryvdh/laravel-ide-helper
```

***

## prod服务器配置

### clone项目
```bash
git clone git@git.domain:user/project-name.git
```

### 执行 composer 脚本
```bash
composer install
composer run-script post-root-package-install
composer run-script post-create-project-cmd
; 配置完 .env 后再执行
composer run-script post-install-cmd
```

### 修改 .env 配置文件
```bash
APP_ENV=production
APP_URL=http://manage.domain.com
DB_DATABASE=project_name
DB_USERNAME=project_name
DB_PASSWORD=project_name
```

### 上传 ssl 证书文件
> gitignore 禁止了证书文件

### 创建项目用户
``` shell
useradd -s /sbin/nologin -g nginx project-name
sh release_prod.sh
```

### 配置服务
```bash
ln -s /path/to/project-name/config/nginx-443_prod.conf /etc/nginx/conf.d/project-name-443.conf
ln -s /path/to/project-name/config/php-fpm-443_prod.conf /etc/php-fpm.d/project-name-443.conf
```

### 创建数据库
```bash
# 创建数据库
CREATE DATABASE `project_name` DEFAULT CHARACTER SET = `utf8mb4` DEFAULT COLLATE = `utf8mb4_unicode_ci`;

# 创建用户
create user 'project_name'@'localhost' identified by 'project_name';

# 查看用户权限
show grants for 'project_name'@'localhost';

# 授权用户
GRANT ALL PRIVILEGES ON project_name.* TO 'project_name'@'localhost';

# 刷新权限
FLUSH PRIVILEGES;
```