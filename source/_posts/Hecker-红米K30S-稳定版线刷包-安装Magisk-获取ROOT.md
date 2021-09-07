---
title: Hecker-红米K30S 稳定版线刷包 安装Magisk 获取ROOT
tags:
  - magisk
  - root
  - K30S
categories:
  - IT
  - Hecker
abbrlink: 738c8e7a
date: 2021-01-16 18:03:19
---
红米K30S 稳定版线刷包 刷入面具 Magisk 获取ROOT
<!-- more -->

### 解锁bootloader
> 根据官方教程解锁手机
[解锁小米手机](https://www.miui.com/unlock/index.html)

[线刷刷机教程](http://www.miui.com/shuaji-393.html)

### 安装面具Magisk
(Magisk)(https://github.com/topjohnwu/Magisk)

### 获取稳定版线刷包

[红米K30S国际稳定版卡刷包 V12.0.9.0.QJDMIXM](https://bigota.d.miui.com/V12.0.9.0.QJDMIXM/miui_APOLLOGlobal_V12.0.9.0.QJDMIXM_5afb52eebd_10.0.zip)

[红米K30S国际稳定版线刷包 V12.0.9.0.QJDMIXM](https://bigota.d.miui.com/V12.0.9.0.QJDMIXM/apollo_global_images_V12.0.9.0.QJDMIXM_20201209.0000.00_10.0_global_8305fec284.tgz)

[红米K30S国际稳定版卡刷包 V12.0.8.0.QJDMIXM](https://bigota.d.miui.com/V12.0.8.0.QJDMIXM/miui_APOLLOGlobal_V12.0.8.0.QJDMIXM_c8052ebd0d_10.0.zip)

[红米K30S国际稳定版线刷包 V12.0.8.0.QJDMIXM](https://bigota.d.miui.com/V12.0.8.0.QJDMIXM/apollo_global_images_V12.0.8.0.QJDMIXM_20201127.0000.00_10.0_global_7b36f2b6c4.tgz)

[红米K30S国际稳定版卡刷包 V12.0.7.0.QJDMIXM](https://bigota.d.miui.com/V12.0.7.0.QJDMIXM/miui_APOLLOGlobal_V12.0.7.0.QJDMIXM_ff2061c1f8_10.0.zip)

[红米K30S国际稳定版线刷包 V12.0.7.0.QJDMIXM](https://bigota.d.miui.com/V12.0.7.0.QJDMIXM/apollo_global_images_V12.0.7.0.QJDMIXM_20201111.0000.00_10.0_global_c2bc57807e.tgz)

[红米K30S欧洲稳定版卡刷包 V12.0.13.0.QJDMIXM](https://bigota.d.miui.com/V12.0.13.0.QJDEUXM/miui_APOLLOEEAGlobal_V12.0.13.0.QJDEUXM_45c42ff803_10.0.zip)

[红米K30S欧洲稳定版线刷包 V12.0.13.0.QJDMIXM](https://bigota.d.miui.com/V12.0.13.0.QJDEUXM/apollo_eea_global_images_V12.0.13.0.QJDEUXM_20201127.0000.00_10.0_eea_6a442adc76.tgz)

[红米K30S国内稳定版卡刷包 V12.0.7.0.QJDCNXM](http://bigota.d.miui.com/V12.0.7.0.QJDCNXM/miui_APOLLO_V12.0.7.0.QJDCNXM_65aae9a857_10.0.zip)

[红米K30S国内稳定版线刷包 V12.0.7.0.QJDCNXM](https://bigota.d.miui.com/V12.0.7.0.QJDCNXM/apollo_images_V12.0.7.0.QJDCNXM_20201204.0000.00_10.0_cn_d46f16477f.tgz)

[红米K30S国内稳定版卡刷包 V12.0.6.0.QJDCNXM](https://bigota.d.miui.com/V12.0.6.0.QJDCNXM/miui_APOLLO_V12.0.6.0.QJDCNXM_9bc709cdbf_10.0.zip)

[红米K30S国内稳定版线刷包 V12.0.6.0.QJDCNXM](https://bigota.d.miui.com/V12.0.6.0.QJDCNXM/apollo_images_V12.0.6.0.QJDCNXM_20201130.0000.00_10.0_cn_d2a2b2c191.tgz)

解压线刷包找到 boot.img 文件

### 修补boot文件
> 将 boot.img 文件放入手机中 Download

> 打开 Magisk 点击 Magisk 安装，选择并修补一个文件，然后找到boot.img文件，选中它。

> 然后等待下载 修补完毕 (如果不能下载，~~你知道的~~)

> 修补后会有一个 magisk_patched_***.img 文件存在于 Download 文件夹里

> 将它拿出来放到电脑上进行刷入

### 刷入 magisk_patched.img 文件
> 我是把 magisk_patched_***.img 文件 重命名成 magisk_patched.img 后再操作的

> 关机状态下 音量- 和 开机键一起按， 进入bootloader模式

> 输入 `fastboot flash boot magisk_patched.img`

> 然后重启 `fastboot reboot`

### 完成了
> 这样shell就变成 # 了， Magisk 也安装成功了
```
adb shell

su - 
```
