title: MAC使用brew安装nginx+php+mysql环境
date: 2015-12-12 15:31:01
tags:
  - php
  - mac
  - mysql
  - nginx
categories:
  - php
---

## 安装 homebrew
>参考资料：http://brew.sh/index_zh-cn.html


```ssh
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

## 安装 nginx

```ssh
brew install nginx
```

### nginx 的操作命令：

```
$打开 nginx
sudo nginx
$重新加载配置|重启|停止|退出 nginx
nginx -s reload|reopen|stop|quit
$测试配置是否有语法错误
nginx -t
```


启动 nginx 后，默认的开启的是8080端口，可以通过修改配置文件来设置端口：

```
vi /usr/local/etc/nginx/nginx.conf
```

默认访问的目录：

```
/usr/local/Cellar/nginx/1.8.0/html
```

### 开机启动

```
mkdir -p ~/Library/LaunchAgents
cp /usr/local/Cellar/nginx/1.8.0/homebrew.mxcl.nginx.plist ~/Library/LaunchAgents/
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist

```

设置权限：

```
sudo chown root:wheel /usr/local/Cellar/nginx/1.8.0/bin/nginx
sudo chmod u+s /usr/local/Cellar/nginx/1.8.0/bin/nginx
```


## 安装mysql

```
brew install mysql
```

### 配置mysql数据库：

```
mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
```


执行完成后就可以在终端中运行 mysql 命令了。

这里需要注意一下，我们可以不需要密码就可以进入 mysql，可以通过一些安全设置、设置用户密码来保证安全性。

设置 mysql 开机启动：

```
mkdir -p ~/Library/LaunchAgents/
cp /usr/local/Cellar/mysql/5.6.17/homebrew.mxcl.mysql.plist ~/Library/LaunchAgents/
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```


mysql 配置文件：

```
/usr/local/Cellar/mysql/5.6.17/my.cnf

```


## 安装 php

MAC本来就自带了 php，但是很多扩展没有安装，所以选择了重新安装php。

首先，我们需要安装第三方程序包。

```
brew tap homebrew/dupes
brew tap josegonzalez/homebrew-php
```

我们可以查看下 brew 下有那些 php 版本

```
brew search php
```

```
brew install php55 --with-imap --with-tidy --with-debug --with-pgsql --with-mysql --with-fpm
```


更多的php选项可以通过以下命令查看：

```
brew options php55
```

由于是重装php，之前系统预装的php还没卸载，因此在终端调用php时，还是以之前系统的php版本做解析，
所以这里需要修改path，指定 php 的解析路径。在~/.bashrc（没有则创建）最后加入一行：

export PATH="$(brew --prefix php55)/bin:$PATH"

执行一下 source 使之生效

```
source ./.profile
```

php 配置文件：

```
/usr/local/etc/php/5.5/php.ini 
```

php-fpm 配置文件：

```
/usr/local/etc/php/5.5/php-fpm.conf
```


启动 php-fpm 的话就直接在终端里执行 "php-fpm"，默认打开 php-fpm 会显示一个状态 shell 出来，
也可以把 php-fpm 的配置文件里的 "daemonize = no" 改为 "daemonize = yes"，就会以后台守护进程的方式启动，
对于刚修改的配置文件，可以执行 "php-fpm -t" 来检测配置有没有问题。

开机启动php-fpm:

mkdir -p ~/Library/LaunchAgents
cp /usr/local/Cellar/php55/5.5.26/homebrew-php.josegonzalez.php55.plist ~/Library/LaunchAgents/
launchctl load -w ~/Library/LaunchAgents/homebrew-php.josegonzalez.php55.plist