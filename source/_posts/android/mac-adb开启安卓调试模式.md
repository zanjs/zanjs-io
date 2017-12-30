---
title: mac adb开启安卓调试模式
date: 2017-12-30 21:39:59
tags:
  - android
  - mac
  - adb
categories:
  - android
---

### mac查看设备信息

```
$ system_profiler SPUSBDataType
```

找到自己对应的 设备信息，其中 `Vendor ID` 中的信息复制下来, 需要保留 。


### 创建 adb_usb

```
$ vi ~/.android/adb_usb.ini
```

黏贴 刚才复制的 `Vendor ID` 


### 关闭并重新启动 `adb`

```
$ adb kill-server
$ adb start-server
```

### 开启安卓手机

开启手机上开发者选项中的 `usb` 调试功能

我的手机是锤子，方法是:进入“设置”——“关于手机(或设备)”——“版本号”或“内核版本”,连续快速点击“版本号”或“内核版本”多次,就可看见“开发者选项”了

### `adb` 命令

查看当前PC端连接有多少设备

```
adb devices
```

