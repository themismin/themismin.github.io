---
title: MAC-brew安装Python配置
tags:
  - Python
  - Python3
categories:
  - IT
  - MAC
abbrlink: c13a576c
date: 2025-02-22 12:49:33
---

# 1. 安装 python

```bash
brew install python
```

# 2. 配置环境变量.zshrc
```bash
export PATH="/usr/local/opt/python@3.13/libexec/bin:$PATH"
```