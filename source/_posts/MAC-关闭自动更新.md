---
title: MAC-关闭自动更新
date: '2021-09-28 13:37:59 - 自动更新'
categories:
  - MAC
abbrlink: 642b2dd1
---

1. 屏蔽网络访问
```
127.0.0.1 swscan.apple.com
127.0.0.1 swcdn.apple.com
127.0.0.1 swdist.apple.com

```

2. 临时清除系统更新标记
```
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0
Killall Dock
```

3. 修改权限(非必需)
```
cd /Volumes/Macintosh\ HD
chmod 644 System/Library/PrivateFrameworks/SoftwareUpdate.framework/Versions/A/Resources/SoftwareUpdateNotificationManager.app/Contents/MacOS/SoftwareUpdateNotificationManager
```