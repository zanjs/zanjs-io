title: 设置webpack实现React项目的hot load
date: 2015-11-22 13:38:36
tags:
	- react
	- webpack
	- javascript
categories:
	- javascript
---


编写了简单的项目: [react-proto m1](https://github.com/MarshalW/react-proto/tree/m1)。用来说明如何设置简单的React开发环境。
通过webpack及其插件 [react-hot-loader](https://github.com/gaearon/react-hot-loader)，实现js/jsx代码修改后，浏览器中的网页能自动加载js。

## 准备工作

安装webpack，见 [webpack getting started](http://webpack.github.io/docs/tutorials/getting-started/)。

## 导入项目及运行

见这里：[README.md#安装与运行](https://github.com/MarshalW/react-proto/blob/master/README.md#%E5%AE%89%E8%A3%85%E4%B8%8E%E8%BF%90%E8%A1%8C)。

运行的截图：

![react](http://marshal.ohtly.com/images/browser-react-hot-loader.png)

相关文件的说明

- webpack.config.js：webpack的配置文件
- server.js：用于开发阶段测试使用的webserver
- index.html：webapp的网页
- scripts目录：存放js脚本
	- index.js：js入口文件
	- App.js：webapp js的入口文件，这里使用了ES6的语法，webpack使用了babel将ES6语法代码转换成ES5的代码