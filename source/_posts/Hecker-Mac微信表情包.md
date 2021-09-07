---
title: Hecker-Mac微信表情包
tags:
  - 微信
  - 表情包
categories:
  - IT
  - Hecker
abbrlink: 84710a72
date: 2021-01-28 17:24:14
---

1. 找到Mac WeChat的sticker文件
```
# 复制文件下的 File文件夹放在桌面（全是表情包）
~/Library/Containers/com.tencent.xinWeChat/Data/Library/Application\ Support/com.tencent.xinWeChat/2.0b4.0.9/{随机值}/Stickers/
```

2. 图片重命名
```
# 安装rename
brew install rename
# 将文件后缀全部设为.gif
rename "s/$/.gif/" ~/Desktop/File/*
# 完成
```