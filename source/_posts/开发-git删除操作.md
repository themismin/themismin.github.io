---
title: 开发-git删除操作
tags:
  - git
abbrlink: '904e2969'
date: 2020-05-25 17:24:52
---

```
git branch -a | grep -v -E 'remotes|master|v8' | xargs git branch -d
git branch -r | grep -v -E 'master|v8' | sed 's/origin\///g'| xargs -I {} git push origin :{}

git tag | grep -v -E 'v8' | xargs -I {} git tag -d {}
git tag | grep -v -E 'v8' | xargs -I {} git push origin :refs/tags/{}
```