---
title: Hecker-magisk 开不了机处理
tags:
  - MIUI
  - root
  - K30S
categories:
  - IT
  - Hecker
abbrlink: ea7188db
date: 2023-11-14 20:31:23
---

> 安装完magisk后 adb shell 授权 su 权限

### 开启zygisk重启之后开不了机
```
magisk --stop zygisk
reboot
```

### 安装模块后开不了机
```
magisk --remove-modules
```