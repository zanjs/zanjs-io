---
title: git 仓库地址修改 ssh与 https 转换
date: 2018-03-01 10:22:42
tags:
  - git
categories:
  - git
---

## 添加仓库地址


```
git remote add github git@github.com:zanjs/frontend-interview.git
```

原本是 `origin`, 这里我改成了  `github` ， 然后提交 `git push github` , 就会提交到新的 `github` 地址上

## 直接修改

```
git remote set-url origin 仓库地址
```

demo

```
git remote set-url git@github.com:zanjs/frontend-interview.git
```

## http 免输入密码

```
$ touch ~/.git-credentials
$ vim ~/.git-credentials
```

添加内容

```
https://{username}:{passwd}@github.com
```

添加 `git` 配置

```
git config --global credential.helper store
```