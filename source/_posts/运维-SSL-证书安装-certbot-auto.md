---
title: 运维-SSL 证书安装 certbot-auto
author: 小敏-blog
tags:
  - SSL证书
  - certbot-auto
categories:
  - IT
  - 运维
abbrlink: 4fd9a168
date: 2019-04-09 16:31:07
---
certbot是专门为Let’s encrypt制作的一个管理证书工具，可以通过它来生成证书管理更新Let’s encrypt证书。
<!-- more -->

## 安装
1. Installing snap on CentOS
```
yum install epel-release
yum install snapd
systemctl enable --now snapd.socket
ln -s /var/lib/snapd/snap /snap
```

```
snap install core
snap refresh core
```

```
yum remove certbot
```

2. Install Certbot
```
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
```

3. Run Certbot
```
certbot certonly --standalone
```

4. Automatic Renewal（nginx服务 80端口）
```
sh -c 'printf "#!/bin/sh\nsystemctl stop nginx\n" > /etc/letsencrypt/renewal-hooks/pre/nginx.sh'
sh -c 'printf "#!/bin/sh\nsystemctl start nginx\n" > /etc/letsencrypt/renewal-hooks/post/nginx.sh'
chmod 755 /etc/letsencrypt/renewal-hooks/pre/nginx.sh
chmod 755 /etc/letsencrypt/renewal-hooks/post/nginx.sh

crontab -e
0 6 1 * * /usr/bin/crontab renew > /dev/null 2>&1 &
```

## 安装（已过期）
1. 下载
```
wget https://dl.eff.org/certbot-auto
```

2. 安装
```
mv certbot-auto /usr/local/bin/certbot-auto
chmod a+x /usr/local/bin/certbot-auto
```

3. 生成证书
```
certbot-auto certonly --standalone -d www.themismin.com --pre-hook "systemctl stop nginx" --post-hook "systemctl start nginx"
```

4. 设置自动更新
```
# 每两个月2号4点16分 域名证书更新
16 4 2 */2 * /usr/local/bin/certbot-auto renew --pre-hook "systemctl stop nginx" --post-hook "systemctl start nginx" > /dev/null 2>&1 &
```

## 问题1
> OCSP check failed 
> OSCP 无法访问 

```
在/etc/hosts中添加

23.32.3.72 ocsp.int-x3.letsencrypt.org
```