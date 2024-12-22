---
title: php-上传文件大小限制
tags:
  - php
  - nginx
  - 文件上传
categories:
  - IT
  - 开发
abbrlink: 887fe8b2
date: 2024-06-17 22:14:22
---

## 报错信息
413 Request Entity Too Large

## 解决方法
修改 php 的配置文件 /etc/php.ini

```
upload_max_filesize = 20M  
post_max_size = 20M
```

修改 Nginx 的配置文件

```
http {  
    ...  
    client_max_body_size 20m;  
    ...  
}
```

重启 Nginx 服务和 php-fpm 服务即可。