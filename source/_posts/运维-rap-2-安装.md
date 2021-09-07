---
title: 运维-rap 2 安装
abbrlink: 26d27558
date: 2019-04-24 14:36:24
tags:
---

## 安装配置

### rap2-delos 安装配置
```
# 下载rap2后端数据API服务器代码
git clone https://github.com/thx/rap2-delos.git

# 初始化
npm install

# 安装全局组件
npm install pm2 -g
npm install typescript -g

# 添加 pm2 系统命令
ln -s /path/nodejs/bin/pm2 /usr/local/bin/

# 修改配置文件
> 生产模式下配置文件地址 src/config/config.prod.ts
> 修改端口、数据库连接

# 编译
npm run build

# 创建表
npm run create-db

# 启动服务
npm start

# 查看 pm2 服务 并 保存开机启动
pm2 list
pm2 save
pm2 startup

# 设置nginx反向代理
```

### rap2-dolores 安装配置

```
# 下载rap2前端静态资源代码
git clone https://github.com/thx/rap2-dolores.git

# 初始化
npm install

# 如果初始化失败 --unsafe-perm 解锁安全模式
npm install --unsafe-perm=true --allow-root

# 修改配置文件
> 生产模式下配置文件地址 src/config/config.prod.js

# 编译
npm run build

# 启动服务
# npm start

# 设置nginx服务，指向/path/rap2-dolores/build
```