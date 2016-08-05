---
title: js 错误捕获
date: 2016-06-23 14:58:09
tags:
	- javascript
	- error
categories:
	- javascript
---

## mozilla 文档

An event handler for the error event. 
Error events are fired at various targets for different kinds of errors:

1. When a JavaScript runtime error (including syntax errors) occurs, 
an error event using interface ErrorEvent is fired at window and window.onerror() is invoked.

2. When a resource (such as an `<img>` or `<script>`) fails to load, 
an error event using interface Event is fired at the element, 
that initiated the load, and the onerror() handler on the element is invoked. 
These error events do not bubble up to window, but (at least in Firefox) can be handled with a single capturing window.addEventListener.


## start

基本可以就是说，window.onerror方法，我们可以写成：

```js
/** 
 * @param {String} errorMessage  错误信息 
 * @param {String} scriptURI   出错的文件 
 * @param {Long}  lineNumber   出错代码的行号 
 * @param {Long}  columnNumber  出错代码的列号 
 * @param {Object} errorObj    错误的详细信息，Anything 
 */
window.onerror = function(errorMessage, scriptURI, lineNumber,columnNumber,errorObj) { 
  // TODO 
}
```

不过使用过程中还得注意兼容性问题，不是所有浏览器都有参数列表中的所有参数，
chrome之类的，都是浏览器标准草案的领跑者，这些个参数用就是了！

于是，可以写一个小Demo来尝试一下：

```html
<!DOCTYPE html> 
<html> 
<head> 
  <title>Js错误捕获</title> 
  <script type="text/javascript"> 
  /** 
   * @param {String} errorMessage  错误信息 
   * @param {String} scriptURI   出错的文件 
   * @param {Long}  lineNumber   出错代码的行号 
   * @param {Long}  columnNumber  出错代码的列号 
   * @param {Object} errorObj    错误的详细信息，Anything 
   */ 
  window.onerror = function(errorMessage, scriptURI, lineNumber,columnNumber,errorObj) { 
    console.log("错误信息：" , errorMessage); 
    console.log("出错文件：" , scriptURI); 
    console.log("出错行号：" , lineNumber); 
    console.log("出错列号：" , columnNumber); 
    console.log("错误详情：" , errorObj); 
  } 
  </script> 
</head> 
<body> 
  <script type="text/javascript" src="error.js"></script> 
</body> 
</html>
```

其中error.js文件中的内容，简单的这样写一句：

用浏览器跑起来以后，打开console，基本就是这样的了：

![](http://files.jb51.net/file_images/article/201601/201601271205084.png)

所以，这些数据都是可以做上报的了。


 当然了，上面的error.js是和html页面同域名下，如果error.js不在同域下，会是怎样的？我们把error.js的引用改一下：

 ```js
 <script type="text/javascript" src="//doitbegin.duapp.com/error.js"></script> 
 ```

 
 再来打开console，我们看到的是这样的：

 ![](http://files.jb51.net/file_images/article/201601/201601271205085.png)

 相当于window.onerror方法只捕获到了一个errorMessage，而且是固定字符串，毫无参考价值。
 查了点资料（[Webkit源码](http://trac.webkit.org/browser/branches/chromium/648/Source/WebCore/dom/ScriptExecutionContext.cpp?rev=77122#L301)），发现在浏览器实现script资源加载的地方，
 是进行了同源策略判断的，如果是非同源资源，errorMessage就被写死为“Script error”了：

 ![](http://files.jb51.net/file_images/article/201601/201601271205086.png)


 好在script标签有一个crossorigin属性，设置它可以显示比较详细的错误信息，我们试着将script标签改一下：

```js
<script type="text/javascript" src="//doitbegin.duapp.com/error.js" crossorigin></script>
```
 刷新页面，这个时候看到console中的输出是这样的：

 ![](http://files.jb51.net/file_images/article/201601/201601271205087.png)

 出现这个error也不意外，既然设置了error.js为crossorigin，
 那error.js的HTTP Response Header也必须设置非同源可访问。
 为了方便设置Header，把error.js做一个小改动，更名为：error-js.php。

 ```php
 <?php 
  header('Access-Control-Allow-Origin:*'); 
  header('Content-type:text/javascript'); 
?> 
throw new Error('出错了'); 
 ```

 此时刷新页面，看到console中的输出就已经正常了，所有信息都能正常捕获：

 ![](http://files.jb51.net/file_images/article/201601/201601271205088.png)


## JavaScript 错误

[Throw、Try 和 Catch](http://www.w3school.com.cn/js/js_errors.asp)

