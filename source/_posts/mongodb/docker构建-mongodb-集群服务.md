---
title: docker构建 mongodb 集群服务
date: 2018-01-16 11:22:27
tags:
  - mongodb
  - docker
  - 数据库
categories:
  - mongodb
---

### 安装

```
docker run -p 27018:27017 -v /root/docker/mongo/data:/data/db  -d --name=mongo361 mongo --bind_ip_all --auth
```

#### 进入容器

```
docker exec -it mongo361 mongo admin
```

创建 所有数据库角色

```
db.createUser({ user: 'zan', pwd: 'zan', roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });
```

[更多角色说明](https://docs.mongodb.com/manual/reference/built-in-roles/#built-in-roles)


认证进入 操作状态

```
db.auth("zan","zan")
```

```
use test
```


### test 只读账号

```
db.createUser({user:"test",pwd:"test",roles:[{role: "read", db: "test"}]})
```

### test1 读写账号

```
db.createUser({user:"test1",pwd:"test",roles:[{role: "readWrite", db: "test"}]})
```
