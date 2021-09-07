---
title: 运维-Centos7 ulimit 设置
tags:
  - ulimit
author: 小敏-mingo
categories:
  - IT
  - 运维
abbrlink: 3dc42c14
date: 2019-08-22 15:29:47
---
ulimit -n是设置当前shell的当前用户所有进程能打开的最大文件数量
<!-- more -->

```
/proc/sys/fs/file-max是系统给出的建议值，系统会计算资源给出一个和合理值，一般跟内存有关系，内存越大，改值越大，但是仅仅是一个建议值，limits.conf的设定完全可以超过/proc/sys/fs/file-max

只有root用户才有权限修改/etc/security/limits.conf

对于非root用户， /etc/security/limits.conf会限制ulimit -n，但是限制不了root用户

对于非root用户，ulimit -n只能越设置越小，root用户则无限制

任何用户对ulimit -n的修改只在当前环境有效，退出后失效，重新登录新来后，ulimit -n由limits.conf决定

如果limits.conf没有做设定，则默认值是1024

当前环境的用户所有进程能打开的最大问价数量由ulimit -n决定
 
 ```

