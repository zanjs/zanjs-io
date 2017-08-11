---
title: mysql 允许远程连接
date: 2017-08-11 09:15:30
tags:
  - Mysqsl
  - MariaDB
categories:
  - Mysqsl
---


## 登陆 `mysql`

```
shell>mysql --user=root -p
```

## 设置远程登陆权限

```
grant all PRIVILEGES on db_test.* to root@'192.168.1.199'  identified by '123'
```

> db_test

表示上面的权限是针对于哪个表的，`db_test` 指的是数据库，后面的 * 表示对于所有的表，由此可以推理出：对于全部数据库的全部表授权为“*.*”，对于某一数据库的全部表授权为“数据库名.*”，对于某一数据库的某一表授权为“数据库名.表名”。

> root 

表示你要给哪个用户授权，这个用户可以是存在的用户，也可以是不存在的用户

> 192.168.1.199

表示允许远程连接的 IP 地址，如果想不限制链接的 IP 则设置为“%”即可

> 123 

为用户的密码

## 执行生效

```
flush privileges
```