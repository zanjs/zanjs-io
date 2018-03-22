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


## package server

```
'react-native run-android' command. And I got the error on emulator "Unable to load script from assets 'index.android.bundle'.Make sure your bundle is packaged correctly or you are running a package server."
```


1. `mkdir android/app/src/main/assets`

2. `react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res`

3. `react-native run-android`


## react-native-image-crop-picker react-link

```
* What went wrong:
A problem occurred configuring project ':app'.
> Could not resolve all dependencies for configuration ':app:_debugApk'.
   > A problem occurred configuring project ':react-native-image-crop-picker'.
      > Could not resolve all dependencies for configuration ':react-native-image-crop-picker:_debugPublishCopy'.
         > Could not find com.github.yalantis:ucrop:2.2.1-native.
           Required by:
               taiZhouTouTiao:react-native-image-crop-picker:unspecified
```


编辑 `build.gradle` 文件 修改如下:

```gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        mavenLocal()
        jcenter()
        maven { url "https://jitpack.io" }
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
    }
}

```