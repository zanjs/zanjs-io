---
title: 为你的Go 加上守护
date: 2017-12-29 11:15:52
tags:
  - golang
  - go
  - supervisord
  - linux
categories:
  - linux
---

## 安装supervisor

[官网地址](http://supervisord.org/index.html)


CentOS install

```bash
yum install python-setuptools
easy_install supervisor
```

Ubuntu install

```
apt-get install supervisor
```

通过这种方式安装后，自动设置为开机启动


安装成功后 生成配置文件

```
sudo echo_supervisord_conf > /etc/supervisord.conf
```

## 添加守护应用的配置文件

在 `/etc/` 下面新建一个文件夹 `supervisorconffile`

在 `supervisorconffile` 中新建个 `demo.conf` 文件

demo.conf

```
[program:demo]
user=root
directory=/root/Applications/go/bin/
command=/root/Applications/go/bin/demo_linux_amd64
autostart=true
autorestart=true
startsecs=10
stdout_logfile=/root/Applications/LogFile/log/demo.log 
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stderr_logfile=/root/Applications/LogFile/err/demo.log
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
stopsignal=INT
[supervisord]
```

### 说明


`directory`：程序的启动目录。
`command`：表示运行的命令，我这是填写的我 `demo` 安装包的原则路径。
`autostart`：表示是否跟随 `supervisor`一起启动。
`autorestart`：如果该程序挂了，是否重新启动。
`stdout_logfile`：终端标准输出重定向文件。
`stderr_logfile`：终端错误输出重定向文件。


## 修改配置文件

```
vi /etc/supervisord.conf
```

拉到文件最下面

加入如下:

```
[include]
files = /etc/supervisorconffile/*.conf
```

## 启动supervisord

```
/usr/bin/supervisord -c /etc/supervisord.conf
```

如果出现错误:

```
[root@ supervisorconffile]# /usr/bin/supervisord -c /etc/supervisord.conf
Error: Another program is already listening on a port that one of our HTTP servers is configured to use.  Shut this program down first before starting supervisord.
For help, use /usr/bin/supervisord -h

```

执行已下操作

```
$ find / -name supervisor.sock
$ unlink /***/supervisor.sock
```

再次执行 `/usr/bin/supervisord -c /etc/supervisord.conf`


## 查看正在守候的进程状态

```
supervisorctl
```

## 手动关闭


停止某一进程 (program_name=你配置中写的程序名称)

```
/usr/bin/supervisorctl stop program_name
```

重启某一进程 (program_name=你配置中写的程序名称)

```
/usr/bin/supervisorctl restart program_name
```

停止全部进程

```
/usr/bin/supervisorctl stop all
```

关闭 `supervisord` 服务

```
kill pid supervisordID
```

## 更新

更新新的配置到 `supervisord `

```
/usr/bin/supervisorctl update
```

重新启动配置中的所有程序 

```
/usr/bin/supervisorctl reload 
```

## 手动启动

```
/usr/bin/supervisord -c /etc/supervisor.conf
```