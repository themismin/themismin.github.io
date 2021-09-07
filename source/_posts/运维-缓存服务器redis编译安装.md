---
title: 运维-缓存服务器redis编译安装
author: 小敏-mingo
tags:
  - 缓存服务器
  - redis
categories:
  - IT
  - 运维
abbrlink: 89f47a3b
date: 2021-06-02 10:30:34
---
Redis是一个使用ANSI C编写的开源、支持网络、基于内存、分布式、可选持久性的键值对存储数据库。从2015年6月开始，Redis的开发由Redis Labs赞助，而2013年5月至2015年6月期间，其开发由Pivotal赞助。[2]在2013年5月之前，其开发由VMware赞助。[3][4]根据月度排行网站DB-Engines.com的数据，Redis是最流行的键值对存储数据库。[5]
<!-- more -->

## 安装基础依赖包
```
yum install -y gcc gcc-c++ make jemalloc-devel epel-release
```

## 下载Redis（ https://redis.io/download ）
> 从官网获取最新版本的下载链接，然后通过wget命令下载
```
wget https://download.redis.io/releases/redis-6.2.4.tar.gz
tar xzf redis-6.2.4.tar.gz
cd redis-6.2.4
make
```

## 编译&安装
```
#进入目录
cd /opt/redis-6.2.4
#编译&安装
make 
make install
```

## 启动redis-server
```
#进入src目录
cd /opt/redis-6.2.4/src
#启动服务端
./redis-server

启动redis客户端测试
#进入src目录
cd /opt/redis-6.2.4/src
#启动客户端
./redis-cli

设置：set key1 value1
获取：get key1
```

## 配置本机外访问
```
修改配置：绑定本机IP&关闭保护模式
#修改配置文件
vi /opt/redis-6.2.4/redis.conf

# 绑定IP地址
bind 0.0.0.0

# 关闭保护模块
protected-mode no
...

# 访问密码
requirepass ******
```

## 开放端口
```
#增加redis端口：6379
firewall-cmd --add-port=6379/tcp --permanent
#重新加载防火墙设置
firewall-cmd --reload

Redis指定配置文件启动
#进入目录
cd /opt/redis-6.2.4
#指定配置文件启动
./src/redis-server redis.conf

Redis客户端连接指定Redis Server
#进入目录
cd /opt/redis-6.2.4
#连接指定Redis Server
./src/redis-cli -h 192.168.11.11
```

## 将Redis配置成为系统服务，以支持开机启动
```
创建Redis服务
#创建服务文件
vim /usr/lib/systemd/system/redis.service

#文件内容
[Unit]
Description=Redis Server
After=network.target

[Service]
ExecStart=/opt/redis-6.2.4/src/redis-server /opt/redis-6.2.4/redis.conf --daemonize no
ExecStop=/opt/redis-6.2.4/src/redis-cli -p 6379 shutdown
Restart=always

[Install]
WantedBy=multi-user.target

设置Redis服务开机启动&开启服务
#设置Redis服务开机启动
systemctl enable redis
#启动Redis服务
systemctl start redis
```

 