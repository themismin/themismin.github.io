---
title: 运维-MariaDB 数据库安装
author: 小敏-blog
tags:
  - 数据库安装
  - MariaDB
categories:
  - IT
  - 运维
abbrlink: 606f56d1
date: 2019-04-09 14:46:38
---
MariaDB是MySQL关系数据库管理系统的一个复刻，由社区开发，有商业支持，旨在继续保持在GNU GPL下开源。
<!-- more -->

## 安装 & 配置
1. 添加源
访问 `https://downloads.mariadb.org/mariadb/repositories` 找到合适的源

2. 添加源
vim /etc/yum.repos.d/MariaDB.repo

```
# MariaDB 10.3 CentOS repository list - created 2019-04-09 06:51 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.3/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

3. yum 安装
```
yum makecache
yum install mariadb-server mariadb
```

4. 设置开机启动
```
systemctl start mariadb
systemctl enable mariadb
```

5. 初始化数据库
```
mysql_secure_installation
```

6. 添加数据库
```
mysql -hlocalhost -uroot -p
CREATE DATABASE `blog` DEFAULT CHARACTER SET = `utf8mb4` DEFAULT COLLATE = `utf8mb4_unicode_ci`;
CREATE USER 'blog'@'localhost' IDENTIFIED BY 'blog';
GRANT ALL PRIVILEGES ON blog.* TO 'blog'@'localhost';
SHOW GRANTS FOR 'blog'@'localhost';
FLUSH PRIVILEGES;
```

```
向super@localhost用户帐户授予所有权限，请使用以下语句。
GRANT ALL ON *.* TO 'super'@'localhost' WITH GRANT OPTION;

ON *.*子句表示MySQL中的所有数据库和所有对象。WITH GRANT OPTION允许super@localhost向其他用户授予权限
```


