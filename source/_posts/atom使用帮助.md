title: atom使用帮助
date: 2015-10-25 18:30:37
tags:
  - atom
  - 前端开发编辑器
  - 开发工具
categories:
  - 开发工具
photos:
  - http://zanjs.b0.upaiyun.com/image/2/47/c7766d97ed2718ad7bd94dfc134e9.png
---

## Atom

一款编辑器入门还是很简单的，学会怎么样创建，打开，编辑，保存文件就行。
剩下的就是慢慢熟悉，Atom 会不断带给你惊喜，如果你想简化或者加快平时工作中的某些任务或者动作，你就可以去搜索一下，Atom 要么本身就为你提供你需要的功能，没有的话，也可以通过现成的插件（Packages）或者自定义的方式解决。

## 安装

如果你顺着我们的路线走过来，你的电脑上应该已经安装好了系统的包管理工具，
Windows 上的 Chocolatey，Mac 上的 Homebrew，Atom 编辑器可以通过包管理工具来安装。

<!--more-->

### Windows

用管理员的身份打开 Powershell，然后用 choco install 去安装 Atom：

```
choco install atom
```
>提示：Atom 编辑器体积挺大，在国内由于网络环境问题，在下载的时候会比较慢，
有时也可能出现不能连接到远程服务器的错误，解决的方法就是，准备 “梯子” 。

### Mac

打开系统的 终端，然后用 Homebrew 的 brew install 命令去安装 Atom：

```
brew install Caskroom/cask/atom
```

在命令行下面安装完 Atom 以后，可以输入 atom ，后面指定一个目录，这样会用 Atom 编辑器打开这个目录。
另外 Atom 编辑器还自带了一个包管理工具叫 apm （Atom Package Manager），
用这个工具可以在命令行下面为编辑器去安装包 （Package） ，包就是 Atom 的插件。

## Packages

Atom 核心的功能是由 Core Packages（核心包） 提供的，另外还有 Community Packages（社区包），
就是由社区成员自己开发并且分享出来的 Package。Atom 可以通过安装这些 Package 来扩展编辑器的功能。
安装 Package 可以在 Atom 的配置界面上去搜索，然后安装，也可以使用 apm 在命令行下面管理编辑器的 Package 。

Packages 列表：https://atom.io/packages

![atom](http://zanjs.b0.upaiyun.com/image/a/0f/ae18039ff2750d0a0d7de123108a4.png)

### 安装包：通过配置界面

- 打开 Atom 编辑器。
- 打开 Atom 的配置界面（Windows：ctrl + ,    Mac：command + , ）。
- 点击边栏上的 Install（安装）。
- 在界面上的 Install Packages 下面，选中 Packages 标签，然后搜索你想要安装的 Package。
- 在搜索结果找到想要的 Package ，点击 Install 安装。

![atom 编辑器](http://zanjs.b0.upaiyun.com/image/5/bb/f3f7dd7cd5ca08f8d523840d1cc2e.png)


### 安装包：通过 apm

- 打开命令行工具，Windows 用 Powershell，Mac 可以使用终端。
- 搜索包用的是 apm search <关键词> 。
- 找到想要的包以后，再用 apm install <包的名字>。

>下面，你可以搜索一个叫 Localization 的包，然后安装一下，这个包会为 Atom 的菜单栏提供一个中文翻译。
下面我们再看一下怎么样去配置与管理包。


## 管理包

打开配置界面，在边栏上选中 Packages ，在这个界面上的 Communtity Packages 区域里，你可以找到自己安装的来自社区成员分享的包。
Core Packages 下面是 Atom 编辑器核心自带的包。

![](http://zanjs.b0.upaiyun.com/image/d/a6/62c3be1ede689b80f9b3d9595fa7f.png)

这里会显示包的名字，还有介绍，不想用的包，可以点击 Disable 按钮禁用它，或者直接点击 Uninstall 卸载掉包，点击 Settings 按钮可以打开包的配置界面，在这个界面上，你可以找到包的主页，说明的文档，可以查看包的源文件，还有相关的配置与快捷键。

下面打开之前安装的 Localization 这个包的配置界面，然后在 Settings 区域里面，在 Current Language 下面的文本框里输入：Chinese - Simplified ，这样会把菜单栏的语言设置成简体中文，如果设置成 Chinese - Traditional，会把菜单栏设置成繁体中文。输入以后，用鼠标点一下浏览器的其它的地方，这样编辑器会保存你的配置。

完成以后，想让设置生效，可以关掉并且重新打开编辑器，或者可以刷新一下编辑器。


>刷新编辑器的快捷键：

```
Mac    ：ctrl + alt + command + L
Windows：ctrl + alt + R
```

## 基础

编辑器没有使用的门槛，打开以后，你就已经知道怎么用了，不过有些小技巧可以了解一下，可以提高工作效率。
先去下载点东西，HTML5Boilerplate（https://html5boilerplate.com/），这个东西可以作为项目的基础，
以后我们会再跟它见面，以后在介绍前端包管理的时候，这个下载的动作可以用命令去做。
下载以后，解压一下，把解压以后的目录重命名成你自己想要创建的项目的名字，
然后用编辑器打开这个项目的目录（`Ｍac：command + O，Windows：ctrl + shift + O`）。

![](http://zanjs.b0.upaiyun.com/image/c/2e/7334fcbcd7a81d474c014bacd030f.png)

编辑器的工作区有两部分组成，左边是编辑器的 Tree View（树形视图），上面会显示你打开的目录里面的东西，
右边是编辑器，在这可以处理打开的文件，最上面是标签栏，点击不同的标签可以打开对应的文件。


## 树形视图

你想打开在树形视图上的文件，创建新的文件或者目录，展开与收起目录，这些动作可以用鼠标完成，或者也可以使用键盘上的按键。
想要在树形视图上操作，你需要把焦点放到树形视图上，切换焦点使用 ctrl + 0 。
你会发现树形视图上的背景颜色会有点变化，具体是什么变化，取决于你的编辑器使用的主题。

查看跟树形视图相关的命令，先确定你的焦点在树形视图上，然后打开命令面板（Command Palette），用快捷键：

```
Mac    ：command + shift + P
Windows：ctrl + shift + P
```

搜索一下 tree view ，列出的就是跟树形视图相关的命令。

![tree view ](http://zanjs.b0.upaiyun.com/image/e/c4/1d687351bddbb3ba974a9f5342657.png)

- 向下移动：↓ 或 J
- 向上移动：↑ 或 K
- 展开目录：→ 或 L
- 收起目录：← 或 H
- 打开文件：enter 回车

>多试几次上面的快捷键，把它变成自己的肌肉记忆。


## 编辑器

先随便打开几个项目里的文件，比如 humans.txt，index.html，还有 css/main.css 。
打开的文件会出现在编辑器的标签栏上，除了用鼠标点击标签可以打开对应的文件，也可以使用快捷键：

打开下一个标签面板

```
Mac    ：alt + command + →
Windows：ctrl + pagedown
```

打开上一个标签面板

```
Mac    ：alt + command + ←
Windows：ctrl + pageup
```

>在 Mac 上，你还可以使用 command + 数字 ，打开对应的标签面板。


关闭标签面板

```
Mac    ：command + W
Windows：ctrl + W
```

## 分离面板
在编辑器上打开的文件可以分离到不同的面板上显示，你可以把编辑器分隔成上，下，左，右，四个部分。
方法是，找到要分离显示的标签面板，鼠标右键点击，然后选择 Split Up，Split Down，Split Left 或者 Split Right。

![](http://zanjs.b0.upaiyun.com/image/2/a4/8a77269fd1060245153c88b0bb187.png)

这些动作也都有对应的快捷键，可以打开命令面板（Mac：command + shift + P，Windows：ctrl + shift + P），
然后搜索 Pane ，这样会显示出面板相关的操作命令。

>分离到上面

```
Mac    ：command + K ↑
Windows：ctrl + K ↑
```

>分离到下面

```
Mac    ：command + K ↓
Windows：ctrl + K ↓
```

>分离到左面

```
Mac    ：command + K ←
Windows：ctrl + K ←
```

>分离到右面

```
Mac    ：command + K →
Windows：ctrl + K →
```

>注意上面这些快捷键的用法，是先按一下 command + K 或者 ctrl + K ，然后松开按键，再按一下上，下，左，右这些箭头按键。


## 查找文件

项目里的文件多了，想找到对应的文件，用鼠标点出这个文件很费事，可以用搜索找到文件。

在已经打开的文件里找到你想要的文件：

```
Mac    ：command + B
Windows：ctrl + B
```

![](http://zanjs.b0.upaiyun.com/image/4/1b/d643f0a52d1e1457321f8b6b61433.png)

在整个项目里找到你需要的文件：

```
Mac    ：command + P
Windows：ctrl + P
```

查找文件里的内容

你可以搜索包含特定内容的文件，比如在当前打开的文件里搜索，或者也可以在整个项目里搜索，
找到以后，可以把搜索的内容替换成新的内容。

在当前打开的文件中搜索

```
Mac    ：command + F
Windows：ctrl + F
```

我想知道有没有查找下一处或者上一处的快捷键，
打开命令面板（Mac：command + shift + P，Windows：ctrl + shift + P），搜索 find ，仔细阅读一下，
你会看到 Find Next ， Find Previous 还有跟它们对应的快捷键。

查找下一个地方

```
Mac    ：command + G
Windows：F3
```

查找上一个地方

```
Mac    ：shift + command + G
Windows：shift + F3
```

在整个项目中搜索

```
Mac    ：shift + command + F
Windows：shift + ctrl + F
```

下面，是我搜索了项目中的 header ，回车以后，会显示出找到的结果，
这个结果显示了包含搜索的内容的文件，还有出现这个内容的位置，点一下，会打开出现这个搜索内容的文件，并且会定位到对应的位置上。

![整个项目](http://zanjs.b0.upaiyun.com/image/6/94/e6a866d83ab1e28d8bf6b0cb81b9e.png)

## 读后感

小伙伴们 辛苦了， 能读到这里，一定吐了吧 ，`O(∩_∩)O哈哈~` , 写的我也快吐了, 排版的不是很好 见谅一下吧。

下面欣赏一下美图吧！

![美图](http://zanjs.b0.upaiyun.com/image/5/09/af43f4834b28c25f60bfcfb3ec523.jpeg)
