---
title: 运维-Laravel Homestead 开发环境
abbrlink: 83809c30
date: 2020-12-21 11:20:55
tags:
  - 虚拟机
  - VBoxManage
  - homestead
---
Laravel Homestead 实际是一个打包好各种 Laravel 开发所需软件和工具的 Vagrant 盒子（关于 Vagrant 盒子的释义请参考 Vagrant 官方文档），该盒子为我们提供了一个优秀的开发环境，有了它，我们不再需要在本地环境安装 PHP、Composer、Nginx、MySQL、Memcached、Redis、Node 等其它工具软件，我们也完全不用再担心误操作搞乱操作系统 —— 因为 Vagrant 盒子是一次性的，如果出现错误，可以在数分钟内销毁并重新创建该 Vagrant 盒子！
<!-- more -->

### 第一步
> 在启动Homestead环境之前，必须安装VirtualBox 5.1，VMWare或Parallels以及Vagrant

### 安装 Homestead Vagrant 盒子

```bash
vagrant box add laravel/homestead
```

### 通过 GitHub 安装 Homestead

> Homestead 盒子作为所有其他 Laravel 项目的主机

```bash
cd ~
git clone https://github.com/laravel/homestead.git Homestead
```

### 创建 Homestead.yaml 配置文件

> 配置文件文件位于 ~/.homestead 目录

```bash
cd ~/Homestead
bash init.sh
```

### 启动 Vagrant Box

```bash
cd ~/Homestead
vagrant up
```

> 更新虚拟机上的配置

```
cd ~/Homestead
vagrant reload --provision
```

### 全局访问 Homestead

> 添加配置命令至 Bash profile 或 zsh profile

```bash
function homestead() {
    ( cd ~/Homestead && vagrant $* )
}
```

### 通过 SSH 连接

> 在 Homestead 目录下通过运行 vagrant ssh 以 SSH 方式连接到虚拟机

```bash
vagrant ssh
```

> 添加命令别名, 可以从任何地方以 SSH 方式连接到 Homestead 虚拟机

```bash
alias vm="ssh vagrant@127.0.0.1 -p 2222"
```

### Composer 及 NPM 镜像配置

> 进入 vagrant 设置 Composer 及 NPM 镜像

```bash
vm
composer config -g repo.packagist composer https://packagist.phpcomposer.com
npm config set registry https://registry.npm.taobao.org
```

### 配置 xdebug 扩展

> 添加 php xdebug 扩展

```
sudo ln -s /etc/php/7.1/mods-available/xdebug.ini /etc/php/7.1/cli/conf.d/20-xdebug.ini
```

### phpstorm 配置 xdebug 调试

> servers 配置勾选 use path mappings 并 填写正确地址

