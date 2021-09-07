---
title: 运维-Jeknins安装
abbrlink: 1363d69a
date: 2019-04-22 20:53:06
tags:
  - jenkins
  - 持续集成
categories:
  - IT
  - 运维
---
Jenkins是一款由Java编写的开源的持续集成工具。
<!-- more -->

## 安装
```
# java 安装
https://www.oracle.com/technetwork/java/javase/downloads/index.html

yum localinstall jdk-8u191-linux-x64.rpm

# 添加源
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

cat /etc/yum.repos.d/jenkins.repo

rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

# yum 安装
yum install jenkins

# 启动
systemctl restart jenkins
systemctl status jenkins
systemctl enable jenkins

# 配置nginx
server {
    listen 80;
    server_name jenkins.mingo.com;

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/jenkins.mingo.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/jenkins.mingo.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

    if ($scheme != "https") {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect     default;
        proxy_set_header   Host              $host;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}

```

## 任务配置
- 安装 Publish over SSH 插件，实现远程自动部署插件
- 安装 Git Parameter 插件，实现分支构建
- 系统管理 - 系统设置 添加 SSH servers 远程服务器
- 新建任务
- 丢弃旧的构建
- 参数化构建过程
- 源码管理-git
- 构建-执行 shell
- 构建后操作-Send build artifacts over SSH
  - 勾选 Make empty dirs
  - 勾选 Clean remote

## 权限配置
- 安装 Role-based Authorization Strategy 插件，实现权限管理
- 系统管理 - 全局安全配置 勾选 Role-Based Strategy
- 系统管理 - Manage and Assign Roles - Manage Roles 管理权限
  - 新增 Global roles 全局角色，勾选 Read 权限
  - 新增 Project roles 项目角色，设置 Pattern `beta-.*` 任务权限，勾选 Build、Cancel、Read、Workspace 权限
- 系统管理 - Manage and Assign Roles - Assign Roles 分配角色
  - 新增 Global roles 用户，分配角色
  - 新增 Item roles 用户，分配角色

