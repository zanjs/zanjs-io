title: Mac OX上安装MongoDb
date: 2015-10-31 08:53:53
tags:
  - mongodb
  - mac
  - 数据库
categories:
  - 数据库
---

## install MongoDb

>更新Homebrew的package数据库，在Mac的终端中输入：

```
$ brew update
```

然后

```
$ brew install mongodb
```

等待MongoDb下载完成。这个比较贴心了，有下载进度百分比。

```
julauddeMacBook-Pro:/ julaud$ brew install mongodb
==> Downloading https://homebrew.bintray.com/bottles/mongodb-3.0.7.yosemite.bott
Already downloaded: /Library/Caches/Homebrew/mongodb-3.0.7.yosemite.bottle.tar.gz
==> Pouring mongodb-3.0.7.yosemite.bottle.tar.gz
==> Caveats
To have launchd start mongodb at login:
  ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
Then to load mongodb now:
  launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
Or, if you don't want/need launchctl, you can just run:
  mongod --config /usr/local/etc/mongod.conf
==> Summary
🍺  /usr/local/Cellar/mongodb/3.0.7: 17 files, 158M
```

## 启动MongoDb

```
🍺  /usr/local/Cellar/mongodb/3.0.7: 17 files, 158M
julauddeMacBook-Pro:/ julaud$ mongod --config /usr/local/etc/mongod.conf
```

运行mongodb执行 mongod命令就可以
mongod --dbpath=/data/db


## 连接MongoDb

```
Last login: Thu Oct 29 20:36:01 on ttys000
julauddeMacBook-Pro:~ julaud$ mongo
MongoDB shell version: 3.0.7
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings:
2015-10-30T22:55:29.874+0800 I CONTROL  [initandlisten]
2015-10-30T22:55:29.874+0800 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
> show dbs
local  0.078GB
> 
```
