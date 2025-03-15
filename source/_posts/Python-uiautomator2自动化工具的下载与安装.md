---
title: Python-uiautomator2自动化工具的下载与安装
tags:
  - douyin
  - 抖音
  - uiautomator
  - uiautomator2
categories:
  - IT
  - Python
abbrlink: 3868f5d7
date: 2025-02-22 12:45:34
---

> 前置操作：MAC-brew安装Python配置

## 1. 安装uiautomator2

```shell
pip install -U uiautomator2
```

> pip安装报错error: externally-managed-environment

通过which python3找到对应的目录，进入目录后，删除EXTERNALLY-MANAGED这个文件即可

## 2. 安装手机端所需的程序atx-agent

```
python -m uiautomator2 init
```

## 3. 安装weditor

> 脚本编写辅助工具，可以快速识别手机上的元素、查看组件信息，并且支持通过Web 界面直接操作手机，以及生成UIAutomator2代码、词试代码，方便测试人员使用

```
pip install -U weditor
```

> 启动
```
python -m weditor
```

## 其他
参考：https://www.cnblogs.com/gancuimian/p/16725664.html