---
title: Hecker-破解基础篇-去LukoolRecorder水印
tags:
  - 破解
  - ResourceHacker
  - LukoolRecorder
  - 去水印
categories:
  - IT
  - Hecker
abbrlink: f7c227dd
date: 2021-09-08 14:54:37
---

### ResourceHacker
1. 在ResourceHacker中打开LukoolRecorder软件
2. 找到水印位图替换成1像素的位图
3. 另存

### OLLYDBG
```
BP FindResourceA
```