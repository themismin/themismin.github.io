---
title: 运维-自助 Git 服务 Gogs 安装
author: 小敏-mingo
tags:
  - Git服务
  - Gogs
categories:
  - IT
  - 运维
abbrlink: 394337ad
date: 2019-04-02 15:04:17
---
一款极易搭建的自助 Git 服务。
<!-- more -->

## 安装
```
# 添加 Gogs 源
wget -O /etc/yum.repos.d/gogs.repo https://dl.packager.io/srv/pkgr/gogs/pkgr/installer/el/7.repo

# 安装 gogs
yum install gogs
```

## 配置
```
# 删除 Gogs 服务配置
rm -rf /etc/systemd/system/gogs*
```
### 新建 Gogs 服务配置
vim /usr/lib/systemd/system/gogs.service
```
[Unit]
Description=Gogs
After=syslog.target
After=network.target
After=mariadb.service mysqld.service postgresql.service memcached.service redis.service

[Service]

Type=simple
ExecStart=/usr/bin/gogs run web
Restart=always

[Install]
WantedBy=multi-user.target
```

### 启动 Gogs 设置开机启动
```
systemctl start gogs
systemctl enable gogs
```

### 添加ngix反向代理

# 添加 Gogs 数据库
```
mysql -hlocalhost -uroot -p
CREATE DATABASE `gogs` DEFAULT CHARACTER SET = `utf8mb4` DEFAULT COLLATE = `utf8mb4_general_ci`;
CREATE USER 'gogs'@'%' IDENTIFIED BY 'gogs';
GRANT ALL PRIVILEGES ON gogs.* TO 'gogs'@'%';
SHOW GRANTS FOR 'gogs'@'%';
FLUSH PRIVILEGES;
exit
```

# 修改 Gogs 配置
```
/etc/gogs/conf/app.ini
```
