---
title: CentOS 7上安装Docker
date: 2018-01-08 12:47:00
tags:
---

## 脚本安装

1. 使用root权限登陆系统。
2. 更新系统包到最新。

```
yum -y update
```

3. 执行Docker安装脚本

```
curl -sSL https://get.docker.com/ | sh
yum install -y docker-selinux
```

4. 启动 `Docker`

```
systemctl start docker.service
```

5. 验证 `docker` 已经正常安装

```
docker run hello-world
```

## yum 安装

1. 添加 `yum` 仓库

```
cat >/etc/yum.repos.d/docker.repo <<-EOF
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
```

2. 安装 `Docker` 包

```
yum install -y docker-engine
```

## 开机自启动

```
systemctl enable docker.service
```