---
title: 开发-git删除操作
tags:
  - git
abbrlink: '904e2969'
date: 2020-05-25 17:24:52
---

```
## 删除本地分支 (除了master)
git branch -a | grep -v -E 'master' | xargs git branch -d
## 删除远程分支 (除了master)
git branch -r | grep -v -E 'master' | sed 's/origin\///g'| xargs -I {} git push origin :{}

## 查看远程tag查看远程仓库所有标签
git ls-remote --tags origin

## 删除本地tag
git tag | xargs git tag -d
## 删除本地tag (除了20241212.1)
git tag | grep -v -E '20241212.1' | xargs -I {} git tag -d {}
## 删除远程tag (除了20241212.1)
git tag | grep -v -E '20241212.1' | xargs -I {} git push origin :refs/tags/{}

## 删除远程分支 (除了master)
git pull -r
```