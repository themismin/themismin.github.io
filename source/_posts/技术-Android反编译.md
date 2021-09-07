---
title: 技术-Android反编译
tags:
  - Android
  - 反编译
categories:
  - IT
  - 开发
abbrlink: 7c0ed167
date: 2019-06-30 12:10:29
---
反编译不是让各位开发者去对一个应用破解搞重装什么的，主要目的是为了促进开发者学习，借鉴好的代码，提升自我开发水平。
<!-- more -->

## 工具
* 下载 AndroidCrackTool
> git 地址 https://github.com/Jermic/Android-Crack-Tool

* 下载需要反编译的 APK

* 反编译
  - 载入APK源文件 > 反编译APK
  - 载入APK源文件 > 提取DEX
  - 载入DEX文件 > DEX2JAR
  - 载入JAR文件 > JDGUI 查看源文件

* 分析
> 一般APK接口都会有加密，需要分析源文件找到加密方式，而盐一般都会放在config.xml文件中
> 打开反编译APK产生的文件夹，找到 res > xml > config.xml 文件，一般里面都会存放着盐
> 打开JAR文件，在JDGUI中搜索 盐 的变量名，分析代码就会找到加密的算法
> 对比接口的加密结果，测试算法是否正确
