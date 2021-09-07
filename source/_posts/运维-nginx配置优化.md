---
title: 运维-nginx配置优化
abbrlink: af500d35
date: 2020-06-30 14:51:45
tags:
---

```
zcat zhenyouhaofang-api_access.log-20200630.gz | awk -F ' ' '{count[$1]++;} END {for(i in count) {print i, count[i]}}' | sort -nk2
zcat zhenyouhaofang-api_access.log-20200630.gz | awk -F ' ' '{count[$1$4]++;} END {for(i in count) {print i, count[i]}}' | sort -nk2


limit_conn_zone $binary_remote_addr zone=perip:10m;
limit_conn_zone $server_name zone=perserver:10m;




limit_conn perip 10;
limit_conn perserver 100;
```