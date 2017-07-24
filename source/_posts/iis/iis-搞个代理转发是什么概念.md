---
title: iis 搞个服务代理转发是什么概念
date: 2017-07-21 13:21:10
tags:
  - iis
  - 反向代理
  - 路由重写
categories:
  - iis
---

习惯了 `nginx` 的 `web` 网关层，遇到 `iis` 服务，给跪了,啥也不说，上去填坑吧!

我们使用 `go` 开发了 一套 短链接服务, 然后目前只能配置在 iis 服务器上 , 所以只好折腾 iis 的代理了

我这次使用 `iis` 的插件 来做 请求转发, 需要下载 `2` 个 微软的东西

[请求代理转发下载](https://www.iis.net/downloads/microsoft/application-request-routing#additionalDownloads)

[url 重写下载](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads)


下载好后 一直下一步就好了, 都安装完后 重启下 iis

### 打开 iis 服务管理后会看到如下界面 记得 双击 `Application Request Routing Cache`

![](/imgs/2017-07-21/approute.jpg)


### 双击右边的 Server Proxy Setting

![](/imgs/2017-07-21/Server-Proxy-Settings.png)

### 勾上 `Enable proxy`

![](/imgs/2017-07-21/Enable.jpg)

然后点击应用

到此 iis 的代理转发就开启了, 但还没完，开启是开启了 但还没配置转发的规则呢。


### 建立站点配置规则


![](/imgs/2017-07-21/1.png)



双击空白规则 进入配置

![](/imgs/2017-07-21/2.png)

### 入站规则如下

请求URL(Requested URL) 选择 与模式匹配(Matches the Pattern )

使用(Using) 选择 正则表达式(Regular Expressions)

模式(Pattern)： 填写 `^(.*) `

勾选 忽略大小写(Ignore case)



1. 
![](/imgs/2017-07-21/3.png)

2. 
![](/imgs/2017-07-21/4.png)

### 展开 条件(Conditions)

逻辑分组(Logical grouping) 选择 任意匹配(Match Any)


点击添加(add)


条件输入(Condition input) 填写  `{HTTP_HOST}` ， `HTTP_HOST` 代表请求头里的 `host`


模式(Pattern) 输入  `^iis.dev$`


### 展开 操作(Action)

重写 URL 输入 `https://cn.bing.com/{R:1}`


到此配置结束，保存这个规则

然后访问 `iis.dev` 就可以看到微软 `bing` 的首页啦






