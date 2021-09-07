---
title: 运维-Api文档管理 ShowDoc 安装
author: 小敏-mingo
tags:
  - Api文档管理
  - ShowDoc
categories:
  - IT
  - 运维
abbrlink: '6957e190'
date: 2019-04-02 11:23:39
---
ShowDoc就是一个非常适合IT团队的在线文档分享工具，它可以加快团队之间沟通的效率。
<!-- more -->

## 安装
### 自动脚本安装 docker
```
# 下载脚本并赋予权限
wget https://www.showdoc.cc/script/showdoc;chmod +x showdoc;
# 默认安装中文版。如果想安装英文版，请加上en参数，如 ./showdoc en
./showdoc
```
### 手动安装
```
yum install php php-gd php-mcrypt php-mbstring php-mysql php-pdo

```
* 下载项目
```
git clone https://github.com/star7th/showdoc
```
* nginx 配置
```
server {
    listen       80;
    server_name  127.0.0.1;
    root         /var/www/html;
    index index.php index.html
    error_page  404              /404.html;
    location = /40x.html {
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
    }
    location ~ \.php$ {
        root           /var/www/html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
    location ~ /\.ht {
        deny  all;
    }
}
```

> 全新安装（具体操作参考文档）并初始化ShowDoc完毕后，进入之前备份的目录。将Sqlite/showdoc.db.php（这是原来的数据库文件），以及Public/Uploads/下的所有文件（这些是上传的图片。如没有图片则可忽略之），全部复制并覆盖到新showdoc目录的相应文件。覆盖后重新给这些文件可写权限。覆盖文件后，用浏览器访问http://x.com/index.php?s=/home/update/db （请将网址更改为你服务器域名或ip）。看到OK字样便证明成功升级.




## 重启宿主机后启动 ShowDoc
```
docker update --restart=always showdoc
```

## 配置 Nginx 反向代理
```
server {
    listen       80;
    listen       [::]:80;

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/doc.domainname.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/doc.domainname.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

    if ($scheme != "https") {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    server_name doc.domainname.com;

    location / {
        proxy_pass http://127.0.0.1:4999;
        proxy_redirect     default;
        proxy_set_header   Host              $host;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

## 其他命令
```
# 启动
./showdoc start 
# 停止
./showdoc stop 
# 重启
./showdoc restart
# 升级showdoc到最新版
./showdoc update
# 卸载showdoc
./showdoc uninstall

# docker 命令
# 启动
docker start showdoc
# 停止
docker stop showdoc
# 重启
docker restart showdoc
```
