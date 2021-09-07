---
title: CentOS-配置 SWAP 分区
author: 小敏-mingo
tags:
  - CentOS
  - CentOS 7
  - 服务器
  - 配置
  - SWAP
  - 交换分区
categories:
  - IT
  - 运维
abbrlink: 7f43682f
date: 2019-02-18 20:13:58
---
Swap分区在系统的物理内存不够用的时候，把物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap分区中，等到那些程序要运行时，再从Swap分区中恢复保存的数据到内存中。
<!-- more -->

## 安装
  > 执行 free -m 查看系统内存情况


  > 生成一个文件作为 SWAP 交换分区
  > /bin/dd if=/dev/zero of=/var/swap bs=1M count=8192


  > 修改文件权限 chmod 0600 /var/swap


  > 将生成的文件格式化成交换分区 /sbin/mkswap /var/swap
  > 启动 SWAP 分区并查看状态 /sbin/swapon /var/swap
  > 显示交换区的使用状况 swapon -s


  > cat /proc/sys/vm/swappiness 查看 SWAP 何时使用
  > 编辑 /etc/sysctl.conf 设置 SWAP 何时使用（vm.swappiness = 30）
  

  > 自动挂载 SWAP 分区 echo "/var/swap swap swap defaults 0 0" >> /etc/fstab
  > 关机重启后，执行 swapon -s 查看交换分区信息或执行 free -m 查看系统内存情况

