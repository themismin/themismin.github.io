---
title: 运维-Supervisor 安装配置
abbrlink: da684dcd
date: 2019-04-24 10:55:13
tags:
  - supervisor
  - 进程管理
categories:
  - IT
  - 运维
---
## 安装
```
yum install supervisor
yum status supervisor
yum enable supervisor
```

## 参数
```
#
```
## 配置
新增 /etc/supervisord.d/laravel-worker.ini
```
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /path/artisan queue:work database --queue=all_comment
autostart=true
autorestart=true
user=forge
numprocs=8
redirect_stderr=true
stdout_logfile=/var/www/zhenyouhaofang/storage/logs/worker.log
```
