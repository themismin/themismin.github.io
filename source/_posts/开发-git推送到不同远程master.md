---
title: 开发-git推送到不同远程master
tags:
  - git
  - push
  - pull
  - remote
abbrlink: cd27b373
date: 2024-04-17 20:22:43
---

## 背景

在开发过程中，我们可能需要将代码推送到不同的远程仓库，例如将代码推送到主仓库和测试仓库。

## 操作步骤

1. 添加主仓库和测试仓库的远程仓库地址

   ```bash
   git remote add origin <主仓库地址>
   git remote add origin-tw <测试1仓库地址>
   git remote add origin-hk <测试2仓库地址>
   ```

2. 推送代码到主仓库

   ```bash
   git push origin master
   ```

3. 推送代码到测试仓库

   ```bash
   git push origin-tw master
   git push origin-hk master
   ```

4. 拉取代码

   ```bash
   git pull origin master
   ```

   ```bash
   git pull origin-tw master
   ```

   ```bash
   git pull origin-hk master
   ```

## 注意事项

1. 确保主仓库和测试仓库的代码是同步的。
2. 确保主仓库和测试仓库的分支是 master。
3. 确保主仓库和测试仓库的远程仓库地址是正确的。

```
git pull <远程主机名> <远程分支>:<本地分支>
git push <远程主机名> <本地分支>:<远程分支>
```
