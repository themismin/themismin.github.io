---
title: 运维-SSR安装
tags:
  - SSR
categories:
  - IT
  - 运维
abbrlink: c8e04e68
date: 2020-12-10 10:55:43
---

### SSR服务器
```
https://github.com/shadowsocksr-backup/shadowsocksr/tree/manyuser
```

### SSR客户端
```
ShadowsocksX-NG
```

### SSR配置
> ./mudb.json 配置
```
[
    {
        "d": 0,
        "enable": 1,
        "method": "chacha20-ietf",
        "obfs": "tls1.2_ticket_auth_compatible",
        "passwd": "passwdxxx",
        "port": 50000,
        "protocol": "auth_sha1_v4",
        "protocol_param": "8",
        "transfer_enable": 9007199254740992,
        "u": 0,
        "user": "userxxx"
    }
]
```

### SSR命令
```
# 开启服务并记录日志
./logrun.sh

# 停止服务
./stop.sh
```
