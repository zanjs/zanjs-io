---
title: react-native android 开发手记
date: 2018-03-20 10:54:14
tags:
  - android
  - react-native
categories:
  - android
---


## 错误如下

```
Error:Execution failed for task ':app:processDebugManifest'.
> Manifest merger failed : Attribute application@allowBackup value=(false) from AndroidManifest.xml:11:7-34
  	is also present at [:react-native-splash-screen] AndroidManifest.xml:12:9-35 value=(true).
  	Suggestion: add 'tools:replace="android:allowBackup"' to <application> element at AndroidManifest.xml:7:5-31:19 to override.
```

找到 `AndroidManifest.xml`

`application` 加入 `tools:replace="android:allowBackup"`

这个时候 `tools` 属性会爆红

在 `manifest` 根标签上加入 `xmlns:tools="http://schemas.android.com/tools"`


[参考官方文档](https://developer.android.com/studio/build/manifest-merge.html)

