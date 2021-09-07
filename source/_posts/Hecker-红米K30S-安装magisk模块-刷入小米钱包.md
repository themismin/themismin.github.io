---
title: Hecker-红米K30S 安装magisk模块 刷入小米钱包
abbrlink: b9f95261
date: 2021-01-20 16:11:27
tags:
---

### 前置条件
1. 刷入 K30S 国际版系统
2. 解锁系统
3. 安装Magisk 获取ROOT
4. 下载 K30S 国内版系统

### Magisk 模块

> Magisk 模块安装列表

* Riru

* Riru - IFW Enhance

* Riru - Clipboard Whitelist

* Riru - Location Report Enabler

* Riru - Enhanced mode for Storage Isolation
> 先安装 [storage-isolation](https://github.com/RikkaApps/StorageRedirect-assets/releases/tag/assets) APK
> 再安装 Riru - Enhanced mode for Storage Isolation 模块

* Riru - EdXposed SandHook
> 先安装模块 [EdXposed-SandHook](https://github.com/ElderDrivers/EdXposed/releases) v0.5+
> 再安装 [EdXposedManager](https://github.com/ElderDrivers/EdXposedManager/releases) APK

* Busybox For Android NDK

### Xposed 模块


### 刷入钱包应用

> 提取 MIUI 钱包
```
git clone https://github.com/kooritea/mipay-extract

cd mipay-extract

# 将 K30S 国内版卡刷包放入 当前文件夹下

chmod +x ./extract.sh

# 提取 extract 里配置的文件
sh ./extract.sh
```

> 文件会提取到当前目录下对应的卡刷包名字的文件夹中
> 不要用项目下的模版，会卡LOGO
> 下载 https://forum.xda-developers.com/t/help-this-zip-is-not-a-magisk-module.4087019/ 第二条帖子的模版文件，解压，并删除 system 下的所有文件
> 复制卡刷包里提取出来的文件 system 下的所有文件到模版文件夹对应位置中。
> 在根目录下修改 module.prop 文件
```
id=AddMiPay
name=Add Mi Pay
version=v1.0
versionCode=1
author=Mingo
description=Add Mi Pay
```

> 在根目录下修改 custormize.sh 没有就创建
```
SKIPUNZIP=1

ui_print "*****************************************************"
ui_print "                 添加钱包和应用商店                  "
ui_print "*****************************************************"

extract "$ZIPFILE" 'module.prop' "$MODPATH"

set_perm_recursive "$MODPATH" 0 0 0755 0644
```

> 在根目录下修改 system.prop 没有就创建
```
ro.se.type=HCE,UICC,eSE
```

> 在当前文件夹下 压缩当前所有文件
```
zip -r MMipay.zip *
```

> 进入手机 打开Magisk 刷入压缩包


## PS

在/system/app路径中提取出Mipay,NextPay,SmartcardService,TSMClient,UPTsmService五个软件包，直接整个文件夹提取出来，然后把TSMClient里面的lib文件夹删除，再在/system/lib64中提取一个libuptsmaddonmi.so文件，到这里提取文件已经结束，接下来就是把提取的文件转移到我们的MIUI国际版上面了

```
brew cask install osxfuse
```

> 挂载 ext4 命令
[fuse-ext2](https://github.com/alperakcan/fuse-ext2)


```
https://canwdev.github.io/manual/android-rom-modify/
```

