---
title: Mysql-替换字段中的内容
author: 小敏-mingo
tags:
  - Mysql
  - REPLACE
  - 字符串替换
categories:
  - IT
  - 开发
abbrlink: 59dda779
date: 2019-02-23 10:50:57
---
Mysql 使用 REPLACE 函数实现字符串替换
`REPLACE(str,from_str,to_str)`
<!-- more -->

## 教程
> 有一张新闻表
```
CREATE TABLE `new` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `content` mediumtext COLLATE utf8mb4_unicode_ci COMMENT '正文',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

> 新闻表数据
```
| id | content |
| ------ | ------ |
| 1 | 123abcxxx111 |
| 2 | 123abcaaaqic |

```

> 替换新闻表中内容里的 abc 字符串为 你好
```
UPDATE new SET cover = REPLACE(content, 'abc', '你好');
```