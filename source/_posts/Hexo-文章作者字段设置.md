---
title: Hexo-文章作者字段设置
author: 小敏-mingo
tags:
  - Hexo 配置
  - Hexo 作者
  - 文章作者
categories:
  - IT
  - Hexo
abbrlink: 5cedc789
date: 2019-02-18 22:59:55
---
...
<!-- more -->

## 新增作者字段
  如本文，新增 `author: 小敏-mingo` 字段
  
  ```
  title: Hexo-文章作者字段设置
  author: 小敏-mingo
  date: 2019-02-18 22:59:55
  tags: ["Hexo 配置","文章作者"]
  categories: ["IT","Hexo"]
  ```

## 编辑模版
  > 找到站点的 themes/next/layout/\_macro 目录下的 post.swig 文件，插入代码如下

```
       #}</{% if theme.seo %}h2{% else %}h1{% endif %}>
        {% endif %}

        <div class="post-meta">


          {# 此位置插入作者字段 #}
          {% if post.author %}
            <span class="post-author" >
              <span class="post-meta-item-text">作者&#58;</span>
              {{ post.author }}
            </span>
            <span class="post-meta-divider">|</span>
          {% endif %}

          <span class="post-time">
            {% if theme.post_meta.created_at %}
              <span class="post-meta-item-icon">
                <i class="fa fa-calendar-o"></i>
```

## 完成以上所有部署后，运行发布命令

```
hexo clean

hexo generate --deploy
```