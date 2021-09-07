---
title: 运维-centos自动更新
tags:
  - yum-cron
  - 更新
categories:
  - IT
  - 运维
abbrlink: 4f7ea0a5
date: 2020-09-29 10:59:10
---
yum-cron程序包使您可以自动将yum命令作为 cron作业运行，以检查，下载和应用更新
<!-- more -->

## 安装、开机启动
```
yum install yum-cron

systemctl enable yum-cron
systemctl start yum-cron

```

## 配置文件
```
# 每日
/etc/yum/yum-cron.conf
# 每小时
/etc/yum/yum-cron-hourly.conf
```

## 配置参数
```
# 只升级 安全更新
update_cmd = security
# 显示更新信息
update_messages = yes
# 下载更新
download_updates = yes
# 安装更新
apply_updates = yes
```

## 查看日志
```
cat /var/log/cron | grep yum-daily
cat /var/log/yum.log | grep Updated
```