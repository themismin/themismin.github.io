---
title: 运维-nodejs安装
abbrlink: 530882a0
date: 2019-08-07 11:24:49
tags:
---

### 下载
https://nodejs.org/zh-cn/download/
```
wget https://nodejs.org/dist/v10.16.2/node-v10.16.2-linux-x64.tar.xz
```

### 解压/安装
```
tar -xvf node-v10.16.2-linux-x64.tar.xz

mv node-v10.16.2-linux-x64 nodejs

ln -s /opt/nodejs/bin/npm /usr/local/bin/
ln -s /opt/nodejs/bin/node /usr/local/bin/

node -v
```


