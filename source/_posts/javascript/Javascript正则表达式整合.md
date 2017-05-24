---
title: Javascript正则表达式整合
date: 2017-05-24 13:14:52
tags:
	- javascript
	- error
categories:
	- javascript
---


### 提取网页标签内容

1. 单个标签提取

```js
let str = `<a class="menu">GitHub</a>`;
let content = str.match(/<a class="menu">([\s\S]+)<\/a>/)[1];

```