---
title: Hexo-添加评论功能
author: 小敏-mingo
tags:
  - Hexo 配置
  - 评论功能
  - Valine
categories:
  - IT
  - Hexo
abbrlink: 7c4d72b1
date: 2019-02-19 21:10:04
---
[Valine](https://valine.js.org/) 诞生于2017年8月7日，是一款基于 [Leancloud](https://leancloud.cn/) 的快速、简洁且高效的无后端评论系统。
注册 LeanCloud 获取 App ID 和 App Key，详情教程见 [Hexo-站点统计配置#LeanCloud 配置使用](http://localhost:4000/posts/3e0d27c4.html) 
<!-- more -->
  
## Valine 配置使用
  > 找到站点的 themes/next 目录下的 \_config.yml 文件，开启 Valine，配置 App ID 和 App Key

```
#hypercomments_id:

# changyan
changyan:
  enable: false
  appid:
  appkey:


# Valine.
# You can get your appid and appkey from https://leancloud.cn
# more info please open https://valine.js.org
valine:
  enable: true
  appid: 1DQSfsF895qG5aRVX3uJtvvL-gzGzoHsz # your leancloud application appid
  appkey: Hnjs1VR4xXd6lECIsgxwCMIE # your leancloud application appkey
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: 说点什么 # comment box placeholder
  avatar: mm # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size


# Support for youyan comments system.
# You can get your uid from http://www.uyan.cc
```

## 完成以上所有部署后，运行发布命令

```
hexo clean && hexo generate --deploy
```