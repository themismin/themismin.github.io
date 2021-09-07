---
title: 运维-v2ray安装教程
categories:
  - IT
  - 运维
tags:
  - 翻墙
  - v2ray
  - 服务器
abbrlink: 87242caa
date: 2021-06-08 14:40:19
---
V2Ray（也简称V2），是Victoria Raymond开发的Project V下的一个工具。Project V是一个工具集合，号称可以帮助其使用者打造专属的基础通信网络。
<!-- more -->

## centos安装V2Ray
> 教程 https://www.v2fly.org/guide/start.html
```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

> 修改开机启动项 /etc/systemd/system/v2ray.service
```
[Unit]
Description=V2Ray Service
Documentation=https://www.v2fly.org/
After=network.target nss-lookup.target

[Service]
User=root # nobody启动失败
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
NoNewPrivileges=true
ExecStart=/usr/local/bin/v2ray -config /usr/local/etc/v2ray/config.json
Restart=on-failure
RestartPreventExitStatus=23

[Install]
WantedBy=multi-user.target
# In case you have a good reason to do so, duplicate this file in the same directory and make your customizes there.
# Or all changes you made will be lost!  # Refer: https://www.freedesktop.org/software/systemd/man/systemd.unit.html
[Service]
ExecStart=
ExecStart=/usr/local/bin/v2ray -config /usr/local/etc/v2ray/config.json
```

## 服务端配置 /usr/local/etc/v2ray/config.json
```
{
  "log": {
    "loglevel": "info",
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log"
  },
  "inbounds": [{
    "port": 443, // 服务器监听端口
    "protocol": "vless", // 协议
    "settings": {
      "clients": [{
        "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      }],
      "decryption": "none"
    },
    "streamSettings": {
      "network": "tcp",
      "security": "tls",
      "tlsSettings": {
        "alpn": [
          "http/1.1"
        ],
        "certificates": [{
          "certificateFile": "/path/to/fullchain.pem",
          "keyFile": "/path/to/privkey.pem"
        }]
      }
    }
  }],
  "outbounds": [{
    "protocol": "freedom"
  }]
}
```

## mac客户端
> 来源 https://github.com/v2fly/fhs-install-v2ray
```
https://github.com/yanue/V2rayU
```


