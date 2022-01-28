---
title: 运维-FFMPEG安装
tags:
  - FFMPEG
  - 服务器
  - 视频
categories:
  - IT
  - 运维
abbrlink: 1b70c2c8
date: 2020-07-20 14:30:47
---
FFmpeg 是一个开放源代码的自由软件，可以运行音频和视频多种格式的录影、转换、流功能，包含了libavcodec——这是一个用于多个项目中音频和视频的解码器库，以及libavformat——一个音频与视频格式转换库
<!-- more -->

## unbunt安装
```
第一步：添加源
sudo add-apt-repository ppa:djcj/hybrid
第二步：更新源
sudo apt-get update
第三步：下载安装
sudo apt-get install ffmpeg
```

## centos最新安装
```
yum-config-manager --add-repo=https://negativo17.org/repos/epel-multimedia.repo
yum-config-manager --disable epel-multimedia
yum install --enablerepo=epel-multimedia ffmpeg ffmpeg-devel
```

## centos安装
```
第一步：添加源
rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm

第二步：下载安装
yum install ffmpeg ffmpeg-devel

```
