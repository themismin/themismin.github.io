---
title: CentOS-Backup-配置服务器
abbrlink: 226071bf
date: 2024-02-17 20:29:36
tags:
---
```bash
# 登录服务器
ssh centoshd1
716381

# 更新
yum update

# 安装 vim gcc
yum install wget vim gcc yum-utils bzip2 lrzsz

# 修改主机名
hostname
hostname centoshd1
vim /etc/hostname
vim /etc/sysconfig/network
vim /etc/hosts

# 重启
reboot

# 生成 SSH 公钥
ssh-keygen

# 设置秘钥登录
cd .ssh/
touch authorized_keys
vim authorized_keys
chmod 400 ~/.ssh/authorized_keys
vim /etc/ssh/sshd_config
systemctl restart sshd

# 安装 firewalld
ifconfig
iptables -L
yum install firewalld firewall-config
systemctl start firewalld
systemctl enable firewalld.service
systemctl status firewalld
firewall-cmd --state
firewall-cmd --get-active-zones
firewall-cmd --get-zone-of-interface=eth0
firewall-cmd --zone=public --list-all
firewall-cmd --complete-reload
systemctl restart firewalld
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --list-all
firewall-cmd --reload
firewall-cmd --zone=public --list-all

# 配置yum
wget -q http://rpms.remirepo.net/enterprise/remi-release-7.rpm
wget -q https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh remi-release-7.rpm epel-release-latest-7.noarch.rpm
rm -rf epel-release-latest-7.noarch.rpm
rm -rf remi-release-7.rpm

# 安装 nginx
yum install nginx
systemctl start nginx
systemctl enable nginx

# 安装 php7.1
yum-config-manager --enable remi-php71
yum install php php-fpm php-mbstring php-xml php-pdo php-mysqlnd php-devel
vim /etc/php.ini
chown root:nginx /var/lib/php/opcache
chown root:nginx /var/lib/php/session
chown root:nginx /var/lib/php/wsdlcache
chown nginx:root /var/log/php-fpm
vim /etc/php-fpm.d/www.conf
systemctl start php-fpm
systemctl enable php-fpm
php --version

# 安装 mariadb
yum install mariadb-server mariadb
systemctl start mariadb
mysql_secure_installation
systemctl enable mariadb

# 安装 git
yum install git

# 安装 composer
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
composer 

# 安装 iftop
yum install iftop
iftop

# 安装 themismin
## 添加数据库
mysql -hlocalhost -uroot -p
CREATE DATABASE themismin;
create user 'themismin'@'%' identified by 'themismin';
GRANT ALL PRIVILEGES ON themismin.* TO 'themismin'@'%';
show grants for 'themismin'@'%';
FLUSH PRIVILEGES;
exit
## 添加项目文件
cd /var/www/
git clone git@git.coding.net:ThemisMin/themismin.git
cd themismin/htdoc/
composer install -vvv
composer run-script post-root-package-install
composer run-script post-create-project-cmd
composer run-script post-install-cmd
vim .env
composer run-script post-install-cmd
php artisan migrate --seed
php artisan spider:site-init
## 设置项目用户
cd /var/www/themismin
useradd -s /sbin/nologin -g nginx themismin
usermod -s /bin/bash themismin
sh release_prod.sh
## 设置项目任务
crontab -u themismin -e

# 安装 friso 分词
cd /opt/
git clone https://git.oschina.net/lionsoul/friso.git
cd /opt/friso/src/
make
make install
cp /usr/lib/libfriso.so /usr/lib64/
## 使用 friso
friso -init /opt/friso/friso.ini

# 安装 scws 分词
cd /opt/
wget http://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2
tar xvjf scws-1.2.3.tar.bz2
cd scws-1.2.3/
./configure --enable-namerule --prefix=/usr/local/scws
make
make install
/usr/local/scws/bin/scws -h
## 添加词典
cd /opt/
wget http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2
tar xvjf scws-dict-chs-utf8.tar.bz2
mv dict.utf8.xdb /usr/local/scws/etc/
## 安装 php-scws 扩展
cd /opt/scws-1.2.3/phpext/
phpize
./configure --help
./configure --with-scws=/usr/local/scws
make
make install
cd /etc/php-zts.d/
vim 40-scws.ini
cp 40-scws.ini /etc/php.d/
php -m
systemctl restart php-fpm

exit
```