---
title: php-imagick安装教程
author: 小敏-mingo
tags:
  - PHP
  - PHP 7
  - imagick
  - ImageMagick
categories:
  - IT
  - PHP
abbrlink: 814eda8
date: 2022-01-27 11:20:21
---
Imagick 是用 ImageMagic API 来创建和修改图像的PHP官方扩展。
<!-- more -->

## 安装
```
yum install ImageMagick ImageMagick-devel
pecl install imagick
```

## 配置 php.ini
```
vim /etc/php.ini

;;;;;;;;;;;;;;;;;;;;;;
; Dynamic Extensions ;
;;;;;;;;;;;;;;;;;;;;;;

extension=imagick
```

