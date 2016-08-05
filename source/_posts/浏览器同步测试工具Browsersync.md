title: 浏览器同步测试工具Browsersync | 前端开发神器
date: 2015-10-26 20:39:40
tags:
  - Browsersync
  - 前端开发工具
  - 开发工具
categories:
  - 开发工具
photos:
  - http://zanjs.b0.upaiyun.com/image/f/75/e7f9489cd69d1625e5326665f247b.jpg
---

## 省时的浏览器同步测试工具

>Browsersync能让浏览器实时、快速响应您的文件更改（html、js、css、sass、less等）并自动刷新页面。
更重要的是Browsersync可以同时在PC、平板、手机等设备下进项调试。
您可以想象一下：“假设您的桌子上有pc、ipad、iphone、android等设备，同时打开了您需要调试的页面，
当您使用browsersync后，您的任何一次代码保存，以上的设备都会同时显示您的改动”。
无论您是前端还是后端工程师，使用它将提高您30%的工作效率。

![同步浏览](http://zanjs.b0.upaiyun.com/image/8/89/55a39bc2736a6f8d306d0c92e1df0.gif)


<!--more-->

有了它，您不用在多个浏览器、多个设备间来回切换，频繁的刷新页面。
更神奇的是您在一个浏览器中滚动页面、点击等行为也会同步到其他浏览器和设备中，这一切还可以通过可视化界面来控制。

![同步测试](http://zanjs.b0.upaiyun.com/image/1/c4/aceebbaf49ede7e599027d0d48565.gif)

## 您不可或缺的测试助手

### 简化操作流程

每个网页在不同设备的浏览器里，测试时间呈指数级增长，无论是PC还是移动端。
曾经我们每改一次的代码，都需要手动去刷新一次页面，查看我们的改动是否正确；
现在，BrowserSync减少了重复的手工任务，这一切都交给BrowserSync去完成，我们只需专注在业务的逻辑里去。

### 工作中您需要它

BrowserSync是建立在网络技术上的，您可以轻松安装在OS X，Windows或Linux上，然后在不同的设备及浏览器里进行调试。
就连UI都可以运行在浏览器 - 尝试在另一台设备上创建第二页面来控制您的BrowserSync。

### 插入到您的工作流程

通过可视化的操作方式或命令行来创建个性化的测试环境，多设备共同响应。
BrowserSync很容易与您的网络平台集成，构建工具和其他Node项目中，例如gulp、grunt。

## 更多功能的加入，完全免费。

### 交互同步

您的滚动，点击，刷新等操作可以在不同浏览器之间同步更新。

### 文件同步

当您改变HTML，CSS，图像和其他项目文件浏览器会自动更新。

### URL历史 NEW

记录您的测试网址，您只需点击一次，就可以在不同设备里访问。

### 同步定制 NEW

切换各个同步设置创建您的首选测试环境。

### 远程督察 NEW

远程调整和正在对连接的设备运行调试网页。

### URL通道 NEW

创建一个安全的公共URL分享您的本地站点，任何设备都可以访问它，并可以响应您的任何改动。

### UI或命令行控制 NEW

使用可视化页面来进行相关设置，也可以使用命令行来完成。

### 浏览器支持

支持PC，平板电脑和手机之间的即时同步。各种文件及时响应，堪称完美。

### 构建工具兼容

可轻松与grunt、gulp等工具配合使用，或包含在其它node项目里。

### 服务于任何本地站点

可以在PHP，ASP，Rails和更多网站运行使用。也可以创建静态环境。

### 安装并运行在任何地方

基于Node.js并支持Windows，MacOS和Linux操作系统，设置只需要5分钟。

### 空闲运行并再利用

浏览器同步是一个开源项目，可根据Apache2.0许可使用或更改。


##  5分钟快速入门

1. 安装 Node.js

BrowserSync是基于Node.js的, 是一个Node模块， 如果您想要快速使用它，也许您需要先安装一下Node.js

[安装适用于Mac OS，Windows和Linux。](http://nodejs.org/download/)

2. 安装 BrowserSync

您可以选择从Node.js的包管理（NPM）[库中](https://npmjs.org/package/browser-sync) 安装BrowserSync。
打开一个终端窗口，运行以下命令：

```
npm install -g browser-sync
```

>您告诉包管理器下载BrowserSync文件，并在全局下安装它们，您可以在所有项目(任何目录)中使用。


当然您也可以结合`gulpjs`或`gruntjs`构建工具来使用，在您需要构建的项目里运行下面的命令:

```
npm install --save-dev browser-sync
```


3. 启动 BrowserSync

一个基本用途是，如果您只希望在对某个css文件进行修改后会同步到浏览器里。
那么您只需要运行命令行工具，进入到该项目（目录）下，并运行相应的命令：

> 静态网站

如果您想要监听.css文件, 您需要使用服务器模式。 BrowserSync 将启动一个小型服务器，并提供一个URL来查看您的网站。

```
// --files 路径是相对于运行该命令的项目（目录）
browser-sync start --server --files "css/*.css"
```

如果您需要监听多个类型的文件，您只需要用逗号隔开。例如我们再加入一个.html文件

```
// --files 路径是相对于运行该命令的项目（目录）
browser-sync start --server --files "css/*.css, *.html"
// 如果你的文件层级比较深，您可以考虑使用 **（表示任意目录）匹配，任意目录下任意.css 或 .html文件。
browser-sync start --server --files "**/*.css, **/*.html"
```

我们做了一个静态例子的示范，您可以下载示例包，文件您可以解压任何盘符的任何目录下，不能是中文路径。
打开您的命令行工具，进入到BrowsersyncExample目录下，运行以下其中一条命令。
Browsersync将创建一个本地服务器并自动打开你的浏览器后访问http://localhost:3000地址，这一切都会在命令行工具里显示。
你也可以查看Browsersync静态示例视频

```
// 监听css文件
browser-sync start --server --files "css/*.css"
// 监听css和html文件
browser-sync start --server --files "css/*.css, *.html"
```


>动态网站

如果您已经有其他本地服务器环境PHP或类似的，您需要使用代理模式。
BrowserSync将通过代理URL(localhost:3000)来查看您的网站。

```
// 主机名可以是ip或域名
browser-sync start --proxy "主机名" "css/*.css"
```

在本地创建了一个PHP服务器环境，并通过绑定Browsersync.cn来访问本地服务器，
使用以下命令方式，Browsersync将提供一个新的地址localhost:3000来访问Browsersync.cn，并监听其css目录下的所有css文件。

```
browser-sync start --proxy "Browsersync.cn" "css/*.css"
```


一点建议

我们建议您结合gulp或grunt来使用，我们这里有详细说明Gulp文档、Grunt文档。
如果您还没有使用gulp或grunt，那么可以通过以上方式创建Browsersync
