---
title: '解决unix:///tmp/supervisor.sock no such file的问题'
date: 2018-04-09 15:02:50
tags:
  supervisord
categories:
  - supervisord
---


- 打开配置文件
  ```sh
  vim /etc/supervisord.conf  
  ```

这里把所有的 `/tmp` 路径改掉，
`/tmp/supervisor.sock` 改成 `/var/run/supervisor.sock`，
`/tmp/supervisord.log` 改成 `/var/log/supervisor.log`，
`/tmp/supervisord.pid` 改成 `/var/run/supervisor.pid`

 要不容易被 `linux` 自动清掉

- 修改权限

```sh
sudo chmod 777 /var/run  
sudo chmod 777 /var/log  
```

如果没改，启动报错 
`IOError: [Errno 13] Permission denied: '/var/log/supervisord.log'`

- 创建 `supervisor.sock`

```
sudo touch /var/run/supervisor.sock
sudo chmod 777 /var/run/supervisor.sock
```

- 启动 `supervisord` ，注意 `stop` 之前的实例或杀死进程

```sh
supervisord 
```

## 重启 

```sh
ps -ef|grep supervisor
```

## 把 `supervisor` 相关的进程都杀掉

```
kill -9 $(ps -ef|grep supervisor | awk '{print $2}')
```

或者

```
kill -9 `ps -ef|grep supervisor | awk '{print $2}'`
```

## 查询端口号占用

```
netstat -tunlp|grep 5980
```