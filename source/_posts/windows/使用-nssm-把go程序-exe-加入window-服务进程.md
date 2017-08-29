---
title: 使用 nssm 把go程序 exe 加入window 服务进程
date: 2017-08-29 11:29:39
tags:
  - nssm
  - exe
  - service
  - windows
categories:
  - windows
---


### install nssm

下载安装 nssm 

[nssm官网](http://nssm.cc/download)

命令行安装 (管理员打开方式)

```ssh
choco install nssm
```

## nssm命令使用


1. 安装服务命令

```
nssm install <servicename>
nssm install <servicename> <program>
nssm install <servicename> <program> [<arguments>]
nssm install AAAAA D:\2017\go\wechat-token.exe
```


2. 删除服务

```
nssm remove
nssm remove <servicename>
nssm remove <servicename> confirm
```


3. 启动、停止服务

```
nssm start <servicename>
nssm stop <servicename>
nssm restart <servicename>
```


4. 查询服务状态

```
nssm status <servicename>
```


5. 服务控制命令

```
nssm pause <servicename>
nssm continue <servicename>
nssm rotate <servicename>
```