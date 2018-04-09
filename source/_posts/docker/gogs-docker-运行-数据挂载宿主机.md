---
title: gogs docker 运行 数据挂载宿主机
date: 2018-04-09 10:32:46
tags:
  - docker
  - gogs
categories:
  - docker
---


## 命令

```
docker run -d --name=gogs -p 10024:22 -p 10083:3000 -v /home/gogs:/data zanjs/gogit
```

