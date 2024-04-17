---
title: mysql-创建用户并设置只读权限
tags:
  - mysql
abbrlink: 64ce2134
date: 2024-04-17 23:34:28
---

- 首先查看mysql中所有的用户

SELECT user,host FROM mysql.user;

- 查看指定用户的权限情况

SELECT * FROM mysql.user WHERE user='root'

- 创建一个用户

CREATE USER '用户名'@'%' IDENTIFIED BY '密码';

- 给用户赋予只读权限。数据库名.* （代表只读那个数据库.后的是表只读那个表*代表所有表）

GRANT SELECT ON 数据库名.* TO '用户名'@'%'; 

- 刷新权限

FLUSH PRIVILEGES;