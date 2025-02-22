---
title: MAC-nodejs的版本管理工具教程
tags:
  - nvm
  - node
  - 版本管理
categories:
  - IT
  - MAC
abbrlink: 8dcc55f2
date: 2025-02-22 11:59:49
---

# MAC-nodejs的版本管理工具教程

## 1. 删除现有node

```
brew uninstall --ignore-dependencies node 
brew uninstall --force node 

sudo rm -rf /usr/local/lib/node_modules
sudo rm -rf /usr/local/include/node

sudo rm /usr/local/bin/node
sudo rm /usr/local/bin/npm
sudo rm /usr/local/bin/npx
```

> 如果你在 .bash_profile、.zshrc、.bashrc 或其他 shell 配置文件中设置了与 Node.js 相关的环境变量，你需要编辑这些文件并移除或注释掉这些变量。

> 验证卸载

```
node -v
npm -v
```

## 2. mac安装nvm

```shell
brew install nvm
mkdir ~/.nvm

- 配置环境

# nvm 切换和管理node版本
export NVM_DIR="$HOME/.nvm"
[ -s "/usr/local/opt/nvm/nvm.sh" ] && \. "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
[ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/usr/local/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```

## 3. 查看nodejs版本

```shell
nvm ls
```

## 4. 切换nodejs版本

```shell
nvm install 14
```

## 5. 使用nodejs版本

```shell
nvm use 18
```

