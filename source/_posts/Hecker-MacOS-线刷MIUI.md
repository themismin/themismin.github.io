---
title: Hecker-MacOS 线刷MIUI
tags:
  - MIUI
  - root
  - K30S
categories:
  - IT
  - Hecker
abbrlink: 7d6336f8
date: 2023-11-13 11:52:14
---
红米K30S Hecker-MacOS 线刷MIUI
<!-- more -->

### 开始 手机进入 fastboot 模式
```
adb reboot fastboot
```

### 解压线刷包 进入线刷包目录，执行下面的命令刷入
```
# 查看设备名
fastboot devices
cd ...
sh {刷机脚本} -s {设备名}
```
PS:几种刷机脚本的选择 
flash_all.sh 擦除全部数据
flash_all_except_storage.sh 擦除全部数据并保留存储数据
flash_all_lock.sh 擦除全部数据并 lock
等待刷入即可
