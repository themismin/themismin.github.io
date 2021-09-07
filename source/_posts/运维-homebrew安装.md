---
title: 运维-homebrew安装
abbrlink: df3214aa
date: 2020-12-14 10:57:23
tags:
---
Homebrew是一款自由及开放源代码的软件包管理系统，用以简化macOS系统上的软件安装过程
<!-- more -->

* 安装homebrew失败：curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused
```
export https_proxy=http://127.0.0.1:1087 http_proxy=http://127.0.0.1:1087 all_proxy=socks5://127.0.0.1:1086
```

* 下载脚本
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```