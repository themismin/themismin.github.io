---
title: Linux-awk统计排序命令
author: 小敏-mingo
tags:
  - Linux
  - Linux 命令
  - awk
  - sort
  - 统计
  - 排序
categories:
  - IT
  - 运维
abbrlink: 9f45d08c
date: 2019-02-21 19:15:09
---
AWK是一个优良的文本处理工具，Linux及Unix环境中现有的功能最强大的数据处理引擎之一。
之所以叫AWK是因为其取了三位创始人 Alfred Aho，Peter Weinberger， 和 Brian Kernighan 的 Family Name 的首字符。
<!-- more -->

## 教程
  > 日志文件如下内容 account.log
  ```
  58.251.121.184 - - [21/Feb/2019:05:43:09 +0800] "GET /lost.php HTTP/1.1" 
  59.36.119.226 - - [21/Feb/2019:05:43:09 +0800] "GET /mysql.php HTTP/1.1" 
  58.251.121.184 - - [21/Feb/2019:05:51:20 +0800] "GET /Updata.php HTTP/1.1" 
  58.251.121.185 - - [21/Feb/2019:05:51:20 +0800] "GET /dbadmin/index.php HTTP/1.1" 
  14.17.3.65 - - [21/Feb/2019:05:51:20 +0800] "GET /l7.php HTTP/1.1" 
  59.36.119.227 - - [21/Feb/2019:05:51:20 +0800] "GET /linux1.php HTTP/1.1" 
  163.177.90.152 - - [21/Feb/2019:05:51:20 +0800] "GET /l8.php HTTP/1.1" 
  14.17.3.64 - - [21/Feb/2019:05:51:20 +0800] "GET /shell.php HTTP/1.1" 
  58.251.121.186 - - [21/Feb/2019:05:51:20 +0800] "GET /zuoss.php HTTP/1.1" 
  183.57.53.177 - - [21/Feb/2019:05:51:21 +0800] "GET /ak.php HTTP/1.1" 
  59.36.119.226 - - [21/Feb/2019:05:54:36 +0800] "GET /7.php HTTP/1.1" 
  14.17.21.58 - - [21/Feb/2019:05:54:36 +0800] "GET /lanke.php HTTP/1.1" 
  183.57.53.177 - - [21/Feb/2019:05:54:37 +0800] "GET /db.init.php HTTP/1.1" 
  14.17.21.58 - - [21/Feb/2019:06:16:19 +0800] "GET /phpMyadmin_bak/index.php HTTP/1.1" "-"
  14.17.21.58 - - [21/Feb/2019:06:16:19 +0800] "GET /pwd/index.php HTTP/1.1" 
  14.17.3.64 - - [21/Feb/2019:06:16:19 +0800] "GET /python.php HTTP/1.1" 
  220.158.136.67 - - [21/Feb/2019:06:44:04 +0800] "GET public/index.php" 
  220.158.136.67 - - [21/Feb/2019:06:44:05 +0800] "GET public/index.php" 
  220.158.136.67 - - [21/Feb/2019:06:44:07 +0800] "GET public/index.php" 
  220.158.136.67 - - [21/Feb/2019:06:44:11 +0800] "GET public/index.php" 
  ```

  > 命令 `awk -F " " '{arr[$7]+=1} END {for(k in arr) {print k,arr[k] | "sort -rnk 2"}}' account.log`
  > `-F " "` 按照空格分割
  > `{arr[$7]++}` 定义一个 arr 数组，统计文件的第七列字段
  > `END` 处理完所有的数据
  > `{for(k in arr) {print k,arr[k] | "sort -rnk 2"}}` 循环输出数组数据，同时通过管道将输出结果发送给sort，按照以相反的顺序，依照数值的大小，第二列字段排序
  
  > 命令结果
  ```
  public/index.php" 4
  /zuoss.php 1
  /shell.php 1
  /python.php 1
  /pwd/index.php 1
  /phpMyadmin_bak/index.php 1
  /mysql.php 1
  /lost.php 1
  /linux1.php 1
  /lanke.php 1
  /l8.php 1
  /l7.php 1
  /dbadmin/index.php 1
  /db.init.php 1
  /ak.php 1
  /Updata.php 1
  /7.php 1
  1
  ```
  