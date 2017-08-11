---
title: Linux上iptables防火墙
date: 2017-08-11 08:56:21
tags:
  - linux
  - iptables
categories:
  - linux
---


## 查看已有的iptables规则，以序号显示

```
iptables -L -n --line-numbers
```


## 开放指定的端口



1. 允许本地回环接口(即运行本机访问本机)

```
iptables -A INPUT -i lo -j ACCEPT
```


1.  允许已建立的或相关连的通行

```
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

```

1. 允许所有本机向外的访问

```
iptables -A OUTPUT -j ACCEPT
```


1.  允许访问22端口

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```


1. 允许访问80端口

```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```


1. 允许访问443端口

```
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```


1. 允许FTP服务的21和20端口

```
iptables -A INPUT -p tcp --dport 21 -j ACCEPT
iptables -A INPUT -p tcp --dport 20 -j ACCEPT
```

1. 如果有其他端口的话，规则也类似，稍微修改上述语句就行

1. 允许ping

```
iptables -A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT
```

1. 禁止其他未允许的规则访问

```
iptables -A INPUT -j REJECT  #（注意：如果22端口未加入允许规则，SSH链接会直接断开。）
iptables -A FORWARD -j REJEC
```



## 屏蔽IP



1. 屏蔽单个IP的命令是

```
iptables -I INPUT -s 123.45.6.7 -j DROP
```


1. 封整个段即从123.0.0.1到123.255.255.254的命令

```
iptables -I INPUT -s 123.0.0.0/8 -j DROP
```


1. 封IP段即从123.45.0.1到123.45.255.254的命令

```
iptables -I INPUT -s 124.45.0.0/16 -j DROP
```


1. 封IP段即从123.45.6.1到123.45.6.254的命令是

```
iptables -I INPUT -s 123.45.6.0/24 -j DROP
```


## 删除对应的DROP规则


```
iptables -D INPUT 5
```