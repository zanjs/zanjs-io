---
title: ubuntu中crontab的用法手记
date: 2016-07-20 09:43:54
tags:
  - linux
  - Ubuntu
  - Crontab
  
categories:
  - Crontab
---

## crontab入门命令

cron是linux中的守护进程，用来定期执行一些任务。基本的命令包括以下几个：

- crontab -e 　编辑该用户的crontab，当指定crontab 不存在时新建。
- crontab -l 　列出该用户的crontab。
- crontab -r 　删除该用户的crontab。
- crontab -u <用户名称> 　指定要设定crontab的用户名称。
- crontab –v 显示上一次编辑的时间（只在某些操作系统上可用）


作为一个新用户，首先需要的操作是，执行命令：

```
$ crontab -e    
```

在linux系统中，不同的用户打开的crontab文件是不同的。当你打开你用户所属的crontab,第一次用这个命令，会让你选择文本编辑器。
我的电脑上显示4中编辑器，如下，我选择是vim.basic。

```
  1. /bin/ed         
  2. /bin/nano      
  3. /usr/bin/vim.basic              
  4. /usr/bin/vim.tiny  
```

如果你编辑时，选择错了编辑器，或者是想更改，可以使用下面的命令进行重新选择：

```
$ select-editor    
```

打开后的crontab内容如下：

```
    # m h  dom mon dow   command      
    */2 * * * * date >> ~/time.log 
```

第二行是我为了测试写的一个定期任务，它的意思是，每隔两分钟就执行 date >> ~/time.log 命令（记录当前时间到time.log文件）。
你可以把它加入你的crontab中，然后保存退出。

保存了crontab之后，我们还需要重启cron来应用这个计划任务。使用以下命令

```
$ sudo service cron restart     
```

## 开启日志功能


修改配置文件：/etc/rsyslog.d/50-default.conf

```
    vim /etc/rsyslog.d/50-default.conf   
```

在配置文件中，把

```
  #cron.*       /var/log/cron.log                
```

前面的注释#号去掉就OK了。


重启rsyslog服务

```
service rsyslog restart  
```


再次查看cron日志

```
ls /var/log/cron.log  
```

备注：这里就有了 crontab 日志。



## 环境变量

`crontab` 有一个坏毛病,就是它总是不会缺省的从用户 `profile` 文件中读取环境变量参数,
经常导致在手工执行某个脚本时是成功的,但是到 `crontab` 中试图让它定期执行时就是会出错.
看了这个就知道怎么修改脚本了,脚本的头上用缺省的 `#!/bin/sh` 就可以,然后然后第一个部分先写这些:

```
    #!/bin/sh
    . /etc/profile
    . ~/.bash_profile
```

## 奇怪