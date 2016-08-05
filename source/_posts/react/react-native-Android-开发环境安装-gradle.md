title: mac 系统下 react-native Android 开发环境安装 gradle 
date: 2016-04-26 13:19:51
tags:
  - react-native
  - react
  - android
categories:
  - react-native
---


## gradle下载地址

>访问很慢，最好翻墙

[下载地址](https://gradle.org/gradle-download/)

当前博主 下载版本是 `DOWNLOAD GRADLE 2.13`

下载是 一个 zip 文件，

## 环境变量配置

文件解压到 `/usr/local/bin/` 目录下

此时文件目录是

```
/usr/local/bin/gradle-2.13-all
```

编辑 `.bash_profile` 或者 `.zshrc`

添加如下内容

```
GRADLE_HOME=/usr/local/bin/gradle-2.13-all;
export GRADLE_HOME
export PATH=$PATH:$GRADLE_HOME/bin
```

然后再在终端 上输入以下命令：

`source ~/.bash_profile` or 'source ~/.zshrc'


到此安装成功

可以通过以下命令来查看是否安装成功。
```
gradle -version
```


## brew install

```
brew install gradle 
```

