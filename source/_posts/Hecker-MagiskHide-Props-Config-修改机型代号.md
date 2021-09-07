---
title: Hecker-MagiskHide Props Config 修改机型代号
tags:
  - MagiskHide
  - K30S
categories:
  - IT
  - Hecker
abbrlink: bde90392
date: 2021-01-18 16:40:25
---

1. 安装 Magisk
2. 安装 MagiskHide Props Config 模块
3. 进入手机命令模式
```
# 进入
adb shell

# 切换 root
su

# 执行 props
props

# 选 1 修改设备指纹
1 - Edit device fingerprint

# 选 1 选择经过认证的指纹
f - Pick a certified fingerprint

# 选择 30 Xiaomi
30 - Xiaomi

# 选择 22 Xiaomi Mi 10T Europe (10) 同系列手机
22 - Xiaomi Mi 10T Europe (10)

# yes 保存
y

# yes 重启使更改生效
y

```