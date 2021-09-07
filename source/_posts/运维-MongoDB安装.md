---
title: 运维-MongoDB安装
abbrlink: 1cdd16d9
date: 2019-08-07 11:20:40
tags:
---
MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。
MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。
<!-- more -->

## 安装
新增 yum 源文件 /etc/yum.repos.d/mongodb-org-4.0.repo
```
[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
```

yum 安装
```
yum install -y mongodb-org
```

验证安装
```
rpm -qa |grep mongodb
rpm -ql mongodb-org-server
```

启动
```
systemctl start mongod.service
systemctl enable mongod.service
```