---
title: Hecker-Android自动开启app
tags:
  - magisk
  - Daily Job Scheduler
  - 定时脚本命令
categories:
  - IT
  - Hecker
abbrlink: acd8cebc
date: 2021-02-02 13:12:52
---

### andorid 命令行启动app
```
adb shell

am start -n package/launch activity

```

### 获取package和launch activity
```
## 手机打开APP

adb shell

logcat | grep -i ActivityManager

```

### 安装 Magisk 的 Daily Job Scheduler 模块
```
## 安装完模块, 添加脚本命令

adb shell

busybox vi /sdcard/djs/djs.conf

## 添加以下命令
0955 am start -n package/launch activity
1822 am start -n package/launch activity

:wq

0955 每天09:55分
1822 每天18:22分
```

### Done