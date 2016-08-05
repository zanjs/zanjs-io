---
title: Ubuntu 启动定时任务Crontab 篇
date: 2016-06-30 10:09:34
tags:
  - linux
  - Ubuntu
  - Crontab
  
categories:
  - Ubuntu
---


Ubuntu 下如何设置定时任务

## Ubuntu启用Crontab

启动cron服务：

```
service cron start
```

如果需要设置为开机时自动启动，则执行

```
sysv-rc-conf --level 35 cron on
```

另外，ubuntu默认没有开启cron执行的日志的，需要做如下3步：

    - 修改rsyslog文件，将/etc/rsyslog.d/50-default.conf 文件中的#cron.*前的#删掉

    - 重启rsyslog服务service rsyslog restart

    - 重启cron服务service cron restart


## Ubuntu 14.04 + 版本的cron中使用notify-send

```ssh
#!/bin/sh

eval "export $(egrep -z DBUS_SESSION_BUS_ADDRESS /proc/$(pgrep -u $LOGNAME gnome-session)/environ)"
notify-send $1
```

[具体参见](http://askubuntu.com/questions/298608/notify-send-doesnt-work-from-crontab/346580#346580)


然后编辑cron配置： crontab -e

```
0 * * * 1-5 sh /home/ronry/bin/cron-notice.sh 休息一下
```


## cron文件语法

```
 分     小时    日       月       星期     命令

0-59    0-23    1-31    1-12     0-6     command     (取值范围,0表示周日一般一行对应一个任务)
```


记住几个特殊符号的含义:

```
    “*”代表取值范围内的数字,
    “/”代表”每”,
    “-”代表从某个数字到某个数字,
    “,”分开几个离散的数字
```


## cron 任务 demo

> **

```
5      *       *         *     *     ls             指定每小时的第5分钟执行一次ls命令
30     5       *         *     *     ls             指定每天的 5:30 执行ls命令
30     7       8         *     *     ls             指定每月8号的7：30分执行ls命令
30     5       8         6     *     ls             指定每年的6月8日5：30执行ls命令
30     6       *         *     0     ls             指定每星期日的6:30执行ls命令[注：0表示星期天，1表示星期1，
```

> -/

```
30     3        10,20     *     *     ls     每月10号及20号的3：30执行ls命令[注：“，”用来连接多个不连续的时段]

25     8-11     *         *     *     ls     每天8-11点的第25分钟执行ls命令[注：“-”用来连接连续的时段]

*/15   *        *         *     *     ls     每15分钟执行一次ls命令 [即每个小时的第0 15 30 45 60分钟执行ls命令 ]

30      6       */10      *     *     ls     每个月中，每隔10天6:30执行一次ls命令[即每月的1、11、21、31日是的6：30执行一次ls 命令。 ]
```



