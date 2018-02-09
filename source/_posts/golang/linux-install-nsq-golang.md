---
title: 'linux install nsq golang '
date: 2018-02-09 17:02:27
tags:
  - golang
  - go
  - supervisord
  - linux
  - nsq
categories:
  - linux
---

## Install

down

```ssh
wget   https://s3.amazonaws.com/bitly-downloads/nsq/nsq-1.0.0-compat.linux-amd64.go1.8.tar.gz
```

```
tar -xzvf nsq-1.0.0-compat.linux-amd64.go1.8.tar.gz
```


## 守护进程

nsqlookupd.conf

```
[program:nsqlookupd]
user=root
directory=/root/go/nsq-1.0.0-compat.linux-amd64.go1.8/bin/
command=/root/go/nsq-1.0.0-compat.linux-amd64.go1.8/bin/nsqlookupd
autostart=true
autorestart=true
startsecs=10
stdout_logfile=/home/go/log/nsqlookupd/log.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stderr_logfile=/home/go/log/nsqlookupd/err.log
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
stopsignal=INT
[supervisord]
```

nsqd.conf


```
[program:nsqd]
user=root
directory=/root/go/nsq-1.0.0-compat.linux-amd64.go1.8/bin/
command=/root/go/nsq-1.0.0-compat.linux-amd64.go1.8/bin/nsqd --lookupd-tcp-address=127.0.0.1:4160
autostart=true
autorestart=true
startsecs=10
stdout_logfile=/home/go/log/nsqd/log.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stderr_logfile=/home/go/log/nsqd/err.log
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
stopsignal=INT
[supervisord]
```

nsqadmin.conf

```
[program:nsqadmin]
user=root
directory=/root/go/nsq-1.0.0-compat.linux-amd64.go1.8/bin/
command=/root/go/nsq-1.0.0-compat.linux-amd64.go1.8/bin/nsqadmin --lookupd-http-address=127.0.0.1:4161
autostart=true
autorestart=true
startsecs=10
stdout_logfile=/home/go/log/nsqadmin/log.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stderr_logfile=/home/go/log/nsqadmin/err.log
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
stopsignal=INT
[supervisord]
```

## 基础概念

- nsqlookupd: 管理nsqd节点拓扑信息并提供最终一致性的发现服务的守护进程

- nsqd: 负责接收、排队、转发消息到客户端的守护进程，并且定时向nsqlookupd服务发送心跳

- nsqadmin：nsqd的web统计界面，可实时查看集群的统计数据和执行一些管理任务

