---
title: Hacker-SSR源码分析
abbrlink: da462d0e
date: 2021-02-07 11:29:50
tags:
---

```
.
├── shadowsocks
│   ├── crypto
│   │   ├── ctypes_libsodium.py
│   │   ├── ctypes_openssl.py
│   │   ├── __init__.py
│   │   ├── openssl.py
│   │   ├── rc4_md5.py
│   │   ├── sodium.py
│   │   ├── table.py
│   │   └── util.py
│   ├── obfsplugin
│   │   ├── auth_chain.py
│   │   ├── auth.py
│   │   ├── http_simple.py
│   │   ├── __init__.py
│   │   ├── obfs_tls.py
│   │   ├── plain.py
│   │   └── verify.py
│   ├── asyncdns.py
│   ├── common.py
│   ├── daemon.py
│   ├── encrypt.py
│   ├── encrypt_test.py
│   ├── eventloop.py
│   ├── __init__.py
│   ├── local.py
│   ├── logrun.sh
│   ├── lru_cache.py
│   ├── manager.py
│   ├── obfs.py
│   ├── ordereddict.py
│   ├── run.sh
│   ├── server.py
│   ├── shell.py
│   ├── stop.sh
│   ├── tail.sh
│   ├── tcprelay.py
│   ├── udprelay.py
│   └── version.py
├── utils
│   ├── fail2ban
│   │   └── shadowsocks.conf
│   ├── autoban.py
│   └── README.md
├── apiconfig.py
├── asyncmgr.py
├── CHANGES
├── config.json
├── configloader.py
├── CONTRIBUTING.md
├── db_transfer.py
├── Dockerfile
├── get-pip.py
├── importloader.py
├── initcfg.bat
├── initcfg.sh
├── initmudbjson.sh
├── LICENSE
├── logrun.sh
├── MANIFEST.in
├── mudb.json
├── mujson_mgr.py
├── mysql.json
├── README.md
├── README.rst
├── run.sh
├── server_pool.py
├── server.py
├── setup_cymysql.sh
├── setup.py
├── ssserver.log
├── stop.sh
├── switchrule.py
├── tail.sh
├── userapiconfig.py
├── user-config.json
└── usermysql.json
```

### 流程
```
./server.py
# 开启线程
MainThread threading.Thread

./db_transfer.py
# 
MuJsonTransfer TransferBase
```
