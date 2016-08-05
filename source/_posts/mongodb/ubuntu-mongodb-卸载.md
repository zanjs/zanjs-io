title: ubuntu mongodb 卸载
date: 2016-01-31 11:44:47
tags:
  - mongodb
  - ubuntu
  - 数据库
categories:
  - 数据库
  - mongodb
---


## 卸载

```ssh
apt-get remove mongodb
```

### 彻底删除

```
sudo apt-get autoremove
sudo apt-get autoclean

dpkg -l |grep ^rc|awk '{print $2}' |tr ["\n"] [" "]|sudo xargs dpkg -P -
```
