---
title: 运维-Android 网络抓包教程 Charles
author: 小敏-mingo
tags:
  - Charles
  - 抓包
categories:
  - IT
  - 运维
abbrlink: c067e142
date: 2019-06-25 14:43:42
---
是一个HTTP代理服务器,HTTP监视器,反转代理服务器，当浏览器连接Charles的代理访问互联网时，Charles可以监控浏览器发送和接收的所有数据。它允许一个开发者查看所有连接互联网的HTTP通信，这些包括request, response和HTTP headers （包含cookies与caching信息）。
<!-- more -->

## 前言
Android 必须解锁 root

## mac安装
略

## Android 证书安装
* 在电脑上保存加密文件 
> 打开 Charles，点击 Help > SSL Proxying > Save Charles Root Certificate

* 计算出 Charles 下载的证书 Hash 值

* Android 系统证书目录
> /system/etc/security/cacerts/
> 其中的每个证书的命名规则如下：<Certificate_Hash>.<Number> 
> 文件名是一个Hash值，而后缀是一个数字。
> 文件名可以用下面的命令计算出来：openssl x509 -subject_hash_old -in <Certificate_File>
> 后缀名的数字是为了防止文件名冲突的，比如如果两个证书算出的Hash值是一样的话，那么一个证书的后缀名数字可以设置成0，而另一个证书的后缀名数字可以设置成1

* 将重命名的证书复制到解锁的手机中
```
# 计算 Hash 值
openssl x509 -subject_hash_old -in charles-ssl-proxying-certificate.pem

# 重命名证书文件
mv charles-ssl-proxying-certificate.pem xxxxxxxx.0

# 测试adb是否安装成功，成功了会出现设备的hash值
adb devices 
adb root

# 禁用系统验证
adb disable-verity 

# 开启 system 写入权限
adb shell
mount -o rw,remount /system
exit

# 推送证书至手机系统证书目录
adb root
adb remount
adb push xxxxxxxx.0 /system/etc/security/cacerts/
```

* 启用证书
> 打开手机，点击 设置 > 更多设置 > 系统安全 > 加密与凭据 > 信任的凭据[系统]

## IOS 证书安装
* 设置网络代理
* 打开浏览器输入chls.pro/ssl下载证书
* 在手机上安装下载的证书，打开 设置->通用->描述文件->charles proxy custom root certificate
* 信任安装的证书，打开 设置->通用->关于本机->证书信任设置->找到 charles proxy custom root certificate 然后信任该证书即可.

## Android 10 证书安装
* 安装 Magisk Manager 的 Move Certificates 模块
* 计算 Hash 值，重命名证书文件
* 复制证书至手机系统证书目录
```
cp /sdcard/Download/xxxxxxxx.0 /system/etc/security/cacerts
```
