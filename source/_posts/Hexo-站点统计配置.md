---
title: Hexo-站点统计配置
author: 小敏-mingo
tags:
  - Hexo 配置
  - 站点统计
  - LeanCloud
  - 不蒜子
categories:
  - IT
  - Hexo
abbrlink: 3e0d27c4
date: 2019-02-14 20:42:38
---
不蒜子是 Bruce 开发的一款轻量级的网页计数器，它的口号是（非官方）
<!-- more -->

## LeanCloud 配置使用
  > 注册 [LeanCloud](https://leancloud.cn/) 账号
  > 创建 blog 应用


  > 创建存储，Class名称必须为Counter，ACL权限为限制写入


  > 设置 Web 安全域名
  
  ```
  http://blog.themismin.com/
  ```
  
  > 查看 App ID 和 App Key


  > 找到站点的 themes/next 目录下的 \_config.yml 文件，开启 LeanCloud Visitors，配置 App ID 和 App Key

```
  color:  fc6423
# ---------------------------------------------------------------

# Show number of visitors to each article.
# You can visit https://leancloud.cn get AppID and AppKey.
leancloud_visitors:
  enable: true
  app_id: 1DQSfsF895qG5aRVX3uJtvvL-gzGzoHsz
  app_key: Hnjs1VR4xXd6lECIsgxwCMIE

# Another tool to show number of visitors to each article.
# visit https://console.firebase.google.com/u/0/ to get apiKey and projectId
# visit https://firebase.google.com/docs/firestore/ to get more information about firestore
firestore:
  enable: false

```

## 不蒜子配置使用
  > 找到站点的 themes/next/layout/\_partials 目录下的 footer.swig 文件，插入代码如下

```
  #}{{ __('footer.theme') }} &mdash; {#
  #}<a class="theme-link" target="_blank" href="https://github.com/iissnan/hexo-theme-next">{#
    #}NexT.{{ theme.scheme }}{#
  #}</a>{% if theme.footer.theme.version %} v{{ theme.version }}{% endif %}{#
#}</div>


{# 此位置插入不蒜子统计代码 #}
<div>
  <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
  <span>本站总访问量<span id="busuanzi_value_site_pv"></span>次</span>
  <span>本站访客数<span id="busuanzi_value_site_uv"></span>次</span>
</div>

{% endif %}

{% if theme.footer.custom_text %}
  <div class="footer-custom">{#
  #}{{ theme.footer.custom_text }}{#

```

## 完成以上所有部署后，运行发布命令

```
hexo clean
hexo generate --deploy
```
