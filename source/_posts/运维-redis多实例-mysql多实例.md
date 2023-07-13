---
title: 运维-redis多实例 mysql多实例
abbrlink: c17c95d4
date: 2023-07-13 16:24:16
tags:
---

## redis 多实例

// 复制默认的配置文件以创建新的配置
cp /etc/redis.conf /etc/redis_6380.conf
cp /etc/redis.conf /etc/redis_6381.conf
cp /etc/redis.conf /etc/redis_6382.conf

// 打开新的配置文件进行编辑（对应修改）
vim /etc/redis_6380.conf
```
# 修改以下几处：
port 6380
pidfile /var/run/redis_6380.pid
daemonize yes
logfile /var/log/redis/redis_6380.log
dbfilename dump_6380.rdb
dir /var/lib/redis
requirepass "xxxxxx"
```

// 启动 redis 实例
/usr/bin/redis-server /etc/redis_6380.conf --supervised systemd
/usr/bin/redis-server /etc/redis_6381.conf --supervised systemd
/usr/bin/redis-server /etc/redis_6382.conf --supervised systemd

// 关闭 redis 实例
/usr/bin/redis-cli -p 6380 -a gp123456 shutdown
/usr/bin/redis-cli -p 6381 -a gp123456 shutdown
/usr/bin/redis-cli -p 6382 -a gp123456 shutdown

## mysql 多实例

// 创建每个实例对应的目录
mkdir -p /data/mysql/3307
mkdir -p /data/mysql/3308
mkdir -p /data/mysql/3309

// 目录所属修改到 MariaDB 用户与用户组
chown -R mysql:mysql /data/mysql/3307
chown -R mysql:mysql /data/mysql/3308
chown -R mysql:mysql /data/mysql/3309

// 初始化数据目录
mysql_install_db --datadir=/data/mysql/3307/data --basedir=/usr --user=mysql
mysql_install_db --datadir=/data/mysql/3308/data --basedir=/usr --user=mysql
mysql_install_db --datadir=/data/mysql/3309/data --basedir=/usr --user=mysql

// MariaDB 配置文件
vim /etc/my.cnf
```
# 添加一下配置
[mysqld_multi]
mysqld     = /usr/bin/mysqld_safe
mysqladmin = /usr/bin/mysqladmin
user       = root
pass       = password

[mysqld3307]
socket     = /data/mysql/3307/mariadb.sock
port       = 3307
pid-file   = /data/mysql/3307/mariadb.pid
datadir    = /data/mysql/3307/data
user       = mysql

[mysqld3308]
socket     = /data/mysql/3308/mariadb.sock
port       = 3308
pid-file   = /data/mysql/3308/mariadb.pid
datadir    = /data/mysql/3308/data
user       = mysql

[mysqld3309]
socket     = /data/mysql/3309/mariadb.sock
port       = 3309
pid-file   = /data/mysql/3309/mariadb.pid
datadir    = /data/mysql/3309/data
user       = mysql
```

// 启动
mysqld_multi start 3307-3309

// 初始化配置 密码设置成 password
mysql_secure_installation -S /data/mysql/3307/mariadb.sock
mysql_secure_installation -S /data/mysql/3308/mariadb.sock
mysql_secure_installation -S /data/mysql/3309/mariadb.sock

// 停止
mysqld_multi stop 3307-3309
