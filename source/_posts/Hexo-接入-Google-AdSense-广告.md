---
title: Hexo-接入 Google AdSense 广告
author: 小敏-mingo
tags:
  - Hexo 配置
  - Google AdSense
  - 广告
categories:
  - IT
  - Hexo
abbrlink: a299b5ad
date: 2019-02-22 19:09:27
---
Google AdSense是一个快速简便的网上赚钱方法，可以让具有一定访问量规模的网站发布商为他们的网站展示与网站内容相关的 Google广告并将网站流量转化为收入。
<!-- more -->

## 前言
需要V屁N的，请自行找梯子

## 步骤
1. 注册 [Google AdSense](https://www.google.com/adsense/start/#/?modal_active=none) 账号

2. 添加广告代码
> 找到站点的 themes/next/layout/\_partials 目录下的 \head.swig 文件，添加 Google AdSense 代码
```
<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9712139945295705",
    enable_page_level_ads: true
  });
</script>
```

3. 静候结果

