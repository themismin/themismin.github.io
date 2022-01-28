---
title: MAC-VBoxManage无法打开homestead
tags:
  - 虚拟机
  - VBoxManage
  - homestead
categories:
  - MAC
abbrlink: 28ce2d58
date: 2021-09-28 11:54:43
---

> 重新启动VirtualBox

```
sudo "/Library/Application Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh" restart
```

> 允许团队标识符 R2H987U7J8
```
启动进入恢复模式：Cmd+R启动时按住
单击实用程序菜单并选择终端。
通过运行以下命令禁用 SIP（​​系统完整性保护）： csrutil disable
执行以下命令：
/usr/sbin/spctl kext-consent add R2H967U7J8
验证团队是否已添加：
/usr/sbin/spctl kext-consent list
关闭终端应用程序并重新启动（正常重新启动，而不是进入恢复模式）

如果可行，请重新启用 SIP 以维护系统安全：
以恢复模式（Command + R）再次启动。
打开终端并运行： csrutil enable
关闭终端并重新启动
```