---
title: Javascript项目笔记
date: 2017-05-24 13:07:56
tags:
	- javascript
	- error
categories:
	- javascript
---


#### 扩展运算符：三个点 …

合并数组

```js
let arr1 = [1, 2], arr2 = [3, 4];
arr1.push(...arr2);  // 把arr2合并到arr1的尾部, arr1改变
arr1.unshift(...arr2);   // 把arr2合并到arr1的顶部, arr1改变
[...arr1, ...arr2];  // 生成一个由arr1和arr2组成的新数组，原数组不变

```

复制对象

```js
let obj = {a: 1};
{...obj, b: 2};     // 返回一个新的对象，{a: 1, b: 2}, obj对象不变
// 等价于下面的用法
Object.assign({}, obj, {b: 2});     // 返回一个新的对象，{a: 1, b: 2},  obj对象不变
// 另外Object.assign 还有一个用法
Object.assign(obj, {b: 2});      // 返回obj对象并且新增加了b属性：{a: 1, b: 2}  obj对象改变
// 由于使用的chrome浏览器还不支持第一种用法，只能演示Object.assign。项目中使用babel转换
```
