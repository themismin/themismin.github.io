---
title: '运维-yum安裝php56, php73, php74, php80多版本共存'
abbrlink: 1b335d6b
date: 2023-07-08 13:15:12
tags:
---

### yum安裝php56, php73, php74, php80多版本共存
1.安装epel：
```
// 查看已安装的PHP，查到后rpm -e 卸载
yum list installed | grep php
yum repolist all | grep php 
yum install epel-release -y
```

2.安装REMI源：
```
rpm -ivh https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/remi-release-7.rpm
```

3.查看可以安装的PHP版本：(54-80版本都有)
```
yum repolist all | grep php

// 安裝 php56 版本
yum install php56 php56-php-fpm php56-php-cli php56-php-bcmath php56-php-gd php56-php-json php56-php-mbstring php56-php-mcrypt php56-php-mysqlnd php56-php-opcache php56-php-pdo php56-php-pecl-crypto php56-php-pecl-mcrypt php56-php-soap php56-php-pecl-zip php56-php-process php56-php-pecl-yaf php56-php-xml php56-php-pecl-swoole4 php56-php-ldap php56-php-pear php56-php-xml php56-php-pecl-imagick php56-php-pecl-wddx

// 安裝 php73 版本
yum install php73 php73-php-fpm php73-php-cli php73-php-bcmath php73-php-gd php73-php-json php73-php-mbstring php73-php-mcrypt php73-php-mysqlnd php73-php-opcache php73-php-pdo php73-php-pecl-crypto php73-php-pecl-mcrypt php73-php-soap php73-php-pecl-zip php73-php-process php73-php-pecl-yaf php73-php-xml php73-php-pecl-swoole4 php73-php-ldap php73-php-pear php73-php-xml php73-php-pecl-imagick php73-php-pecl-wddx

// 安裝 php74 版本
yum install php74 php74-php-fpm php74-php-cli php74-php-bcmath php74-php-gd php74-php-json php74-php-mbstring php74-php-mcrypt php74-php-mysqlnd php74-php-opcache php74-php-pdo php74-php-pecl-crypto php74-php-pecl-mcrypt php74-php-soap php74-php-pecl-zip php74-php-process php74-php-pecl-yaf php74-php-xml php74-php-pecl-swoole4 php74-php-ldap php74-php-pear php74-php-xml php74-php-pecl-imagick php74-php-pecl-wddx

// 安裝 php80 版本
yum install php80 php80-php-fpm php80-php-cli php80-php-bcmath php80-php-gd php80-php-json php80-php-mbstring php80-php-mcrypt php80-php-mysqlnd php80-php-opcache php80-php-pdo php80-php-pecl-crypto php80-php-pecl-mcrypt php80-php-soap php80-php-pecl-zip php80-php-process php80-php-pecl-yaf php80-php-xml php80-php-pecl-swoole4 php80-php-ldap php80-php-pear php80-php-xml php80-php-pecl-imagick php80-php-pecl-wddx

// 安裝 php81 版本
yum install php81 php81-php-fpm php81-php-cli php81-php-bcmath php81-php-gd php81-php-json php81-php-mbstring php81-php-mcrypt php81-php-mysqlnd php81-php-opcache php81-php-pdo php81-php-pecl-crypto php81-php-pecl-mcrypt php81-php-soap php81-php-pecl-zip php81-php-process php81-php-pecl-yaf php81-php-xml php81-php-pecl-swoole4 php81-php-ldap php81-php-pear php81-php-xml php81-php-pecl-imagick php81-php-pecl-wddx
```

3-1.卸载PHP版本
```
// 卸载安装包
yum remove php56 php56-php-fpm php56-php-cli php56-php-bcmath php56-php-gd php56-php-json php56-php-mbstring php56-php-mcrypt php56-php-mysqlnd php56-php-opcache php56-php-pdo php56-php-pecl-crypto php56-php-pecl-mcrypt php56-php-soap php56-php-pecl-zip php56-php-process php56-php-pecl-yaf php56-php-xml php56-php-pecl-swoole4 php56-php-ldap php56-php-pear php56-php-xml php56-php-pecl-imagick php56-php-pecl-wddx

// 查看rpm包
rpm -qa|grep php56
// 删除rpm包
rpm -e php56-runtime-5.6-1.el7.remi.x86_64

ls -al /opt/remi/
ls -al /etc/opt/remi/
```

4. 启动服务
安裝好之後爲了PHP版本能夠共存還需要更改不同的php版本端口

// 在路徑下能夠找到相應的版本
```
ls /etc/opt/remi/
```

// 修改 php.ini 配置
vim /etc/opt/remi/php73/php.ini
    # 开启整站 zlib 压缩输出
    zlib.output_compression = Off
    # 修改 zlib 压缩比
    zlib.output_compression_level = 5
    # 关闭 PATH_INFO
    cgi.fix_pathinfo = 0
    # 设置时区
    date.timezone = Asia/Shanghai

vim /etc/opt/remi/php74/php.ini
    # 开启整站 zlib 压缩输出
    zlib.output_compression = Off
    # 修改 zlib 压缩比
    zlib.output_compression_level = 5
    # 关闭 PATH_INFO
    cgi.fix_pathinfo = 0
    # 设置时区
    date.timezone = Asia/Shanghai

vim /etc/opt/remi/php80/php.ini
    # 开启整站 zlib 压缩输出
    zlib.output_compression = Off
    # 修改 zlib 压缩比
    zlib.output_compression_level = 5
    # 关闭 PATH_INFO
    cgi.fix_pathinfo = 0
    # 设置时区
    date.timezone = Asia/Shanghai

vim /etc/opt/remi/php81/php.ini
    # 开启整站 zlib 压缩输出
    zlib.output_compression = Off
    # 修改 zlib 压缩比
    zlib.output_compression_level = 5
    # 关闭 PATH_INFO
    cgi.fix_pathinfo = 0
    # 设置时区
    date.timezone = Asia/Shanghai
```

// 編輯配置文件修改listen端口（端口可自定义）
vim /etc/opt/remi/php73/php-fpm.d/www.conf
    # 监听
    listen = 127.0.0.1:9073
    # 设置进程所属用户及用户组
    user = nginx
    group = nginx
    # 设置 unix socket 权限
    listen.owner = nginx
    listen.group = nginx
    listen.mode = 0660
vim /etc/opt/remi/php74/php-fpm.d/www.conf
    # 监听
    listen = 127.0.0.1:9074
    # 设置进程所属用户及用户组
    user = nginx
    group = nginx
    # 设置 unix socket 权限
    listen.owner = nginx
    listen.group = nginx
    listen.mode = 0660
vim /etc/opt/remi/php80/php-fpm.d/www.conf
    # 监听
    listen = 127.0.0.1:9080
    # 设置进程所属用户及用户组
    user = nginx
    group = nginx
    # 设置 unix socket 权限
    listen.owner = nginx
    listen.group = nginx
    listen.mode = 0660
vim /etc/opt/remi/php81/php-fpm.d/www.conf
    # 监听
    listen = 127.0.0.1:9081
    # 设置进程所属用户及用户组
    user = nginx
    group = nginx
    # 设置 unix socket 权限
    listen.owner = nginx
    listen.group = nginx
    listen.mode = 0660
```

// 修改相关文件的所属
```
chown root:nginx /var/opt/remi/php73/lib/php/opcache
chown root:nginx /var/opt/remi/php73/lib/php/session
chown root:nginx /var/opt/remi/php73/lib/php/wsdlcache
chown nginx:root /var/opt/remi/php73/log/php-fpm

chown root:nginx /var/opt/remi/php74/lib/php/opcache
chown root:nginx /var/opt/remi/php74/lib/php/session
chown root:nginx /var/opt/remi/php74/lib/php/wsdlcache
chown nginx:root /var/opt/remi/php74/log/php-fpm

chown root:nginx /var/opt/remi/php80/lib/php/opcache
chown root:nginx /var/opt/remi/php80/lib/php/session
chown root:nginx /var/opt/remi/php80/lib/php/wsdlcache
chown nginx:root /var/opt/remi/php80/log/php-fpm

chown root:nginx /var/opt/remi/php81/lib/php/opcache
chown root:nginx /var/opt/remi/php81/lib/php/session
chown root:nginx /var/opt/remi/php81/lib/php/wsdlcache
chown nginx:root /var/opt/remi/php81/log/php-fpm
```

// 启动服务
```
systemctl restart php73-php-fpm
systemctl enable php73-php-fpm

systemctl restart php74-php-fpm
systemctl enable php74-php-fpm

systemctl restart php80-php-fpm
systemctl enable php80-php-fpm

systemctl restart php81-php-fpm
systemctl enable php81-php-fpm
```
