---
title: Hecker-小米8安装twrp及刷入magisk xposed
tags:
  - magisk
categories:
  - IT
  - Hecker
abbrlink: cd3358e6
date: 2019-09-10 11:49:57
---

## 教程

1. 开发版 + root
```
1. 卡刷 miui 开发版
2. 进入安全中心开启 root 权限
3. 解锁 system 分区

adb root
adb disable-verity
adb reboot

```

2. 安装 twrp
```
# 下载 recovery TWRP 包
https://twrp.me/lg/lgnexus5.html

# 进入 bootloader 模式
adb reboot bootloader

# 解锁 - 小米手机需要官方申请解锁
fastboot oem unlock

# 刷入刚下载的 TWRP 包
fastboot devices
fastboot flash recovery twrp.img
# 小米手机不要直接重启，会被原厂覆盖，可直接进入recovery模式 关机键+音量加
fastboot reboot
```

3. 下载google ota
```
# 下载 - google ota 不想刷 ota 可跳过
https://developers.google.com/android/images
Factory Images(线刷包)
Full OTA Images(卡刷包)

# 将 nexus 5 Full OTA Images 放入 sd 卡 - google ota 不想刷 ota 可跳过
adb push ./hammerhead-m4b30x-factory-10cfaa5c.zip /sdcard/downloaded_rom
```

4. 卡刷 magisk 安装 xposed
```
# 进入 recovery 模式刷入卡刷包
adb reboot recovery

# 推送 Magisk.zip Magisk-uninstaller.zip 
adb push ./Magisk.zip /sdcard/downloaded_rom
# TWRP 安装 zip

# 安装 MagiskManager.apk 
adb reboot
adb install MagiskManager.apk

# 安装 Magisk Riru-Core Riru-EdXposed 模块

# 安装 EdXposedManager.apk
adb install EdXposedManager.apk
```