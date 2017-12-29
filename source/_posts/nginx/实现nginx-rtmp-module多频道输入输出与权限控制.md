---
title: 直播方案 多频道输入输出与权限控制
date: 2017-12-19 17:26:20
tags: 
  - live
  - nginx
categories:
  - nginx
---

这里使用得是 ` nginx-rtmp-module`

# 权限控制方面

1. live配置的publish_notify部分 [link](https://github.com/arut/nginx-rtmp-module/wiki/Directives#publish_notify)
 

 2. publish_notify中Notify的配置部分 [link](https://github.com/arut/nginx-rtmp-module/wiki/Directives#notify)


 ## live的publish_notify

 所谓的 `publish_notify是涉及publish_notify` 默认是 `off` 的，
 
 主要涉及推送的过程中一些事件。

开启 `publish_notify` 即可进行 `Notify` 的配置操作。

```
publish_notify on;
```


## Notify的配置


`Notify` 的配置相关是涉及直播的事件并执行回调代码。

> 比如：推流链接、直播开启、直播结束状态，然后异步调用http的链接，进行一些逻辑的处理。

主要的配置参数有下面这些：

1. `on_connect`
2. `on_play`
3. `on_publish`
4. `on_done`
5. `on_play_done`
1. `on_publish_done`
1. `on_record_done`
1. `on_update`
1. ...


从上面的配置参数可以看出，能够触发连接、直播、输出、直播结束等等，从而能够进行权限验证。

比如，当触发推流的时候，通过 on_publish http://live.zanjs.com/uri 进行权限控制，接收相关参数并进行控制，如果用户不存在，则不允许推流。


# 多频道输入输出