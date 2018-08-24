---
title: Docker容器开机自动启动
date: 2018-08-24 09:05:51
tags:
 - docker
---


##  docker run启动容器 使用--restart参数来设置

```s
docker run -m 512m --memory-swap 1G -it -p 6379:6379 --restart=always 
--name redis -d redis
```


### --restart具体参数值详细信息


-  no -  容器退出时，不重启容器
-  on-failure - 只有在非0状态退出时才从新启动容器
-  always - 无论退出状态是如何，都重启容器



还可以在使用 `on-failure` 策略时，指定 `Docker` 将尝试重新启动容器的最大次数。
默认情况下，`Docker` 将尝试永远重新启动容器。


```s
sudo docker run --restart=on-failure:10 redis
```


如果创建时未指定 `--restart=always` ,可通过 `update` 命令


```s
docker update --restart=always xxx
```