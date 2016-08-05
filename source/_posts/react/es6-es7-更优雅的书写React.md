---
title: es6 es7 更优雅的书写React
date: 2016-06-12 13:19:49
tags:
  - react
  - es6
  - jsx
categories:
  - react
---


目前我们想使用 es6 编写 React 应用时，常用到Babel这个编译工具，
在项目根目录下 建立 `.babelrc` ,内容如下:

```
{
    "presets": [
      "es2015",
      "react",
      "stage-0"
    ],
    "plugins": []
}
```




我们现在来说明下这个配置文件是什么意思。
首先，这个配置文件是针对babel 6的。
Babel 6做了一系列模块化，不像Babel 5一样把所有的内容都加载。
比如需要编译ES6，我们需要设置presets为"es2015"，也就是预先加载es6编译的相关模块，
如果需要编译jsx，需要预先加载"react"这个模块。
那问题来了，这个"stage-0"又代表什么呢？ 有了"react-0"，是否又有诸如"stage-1", "stage-2"等等呢？


事实上， ”stage-0"是对ES7一些提案的支持，Babel通过插件的方式引入，让Babel可以编译ES7代码。
当然由于ES7没有定下来，所以这些功能随时肯能被废弃掉的。现在我们来一一分析里面都有什么。

## 法力无边的stage-0

为什么说“stage-0” 法力无边呢，因为它包含stage-1, stage-2以及stage-3的所有功能，
同时还另外支持如下两个功能插件：

- [transform-do-expressions](https://babeljs.io/docs/plugins/transform-do-expressions)

- [transform-function-bind](https://babeljs.io/docs/plugins/transform-function-bind)


用过React的同学可能知道，jsx对条件表达式支持的不是太好，
你不能很方便的使用if/else表达式，要么你使用三元表达，要么用函数。
例如你不能写如下的代码：


```js
var App = React.createClass({

    render(){
        let { color } = this.props;

        return (
            <div className="parents">
                {
                    if(color == 'blue') { 
                        <BlueComponent/>; 
                    }else if(color == 'red') { 
                        <RedComponent/>; 
                    }else { 
                        <GreenComponent/>; }
                    }
                }
            </div>
        )
    }
})
```

在React中你只能写成这样：

```js
var App = React.createClass({

    render(){
        let { color } = this.props;


        const getColoredComponent = color => {
            if(color === 'blue') { return <BlueComponent/>; }
            if(color === 'red') { return <RedComponent/>; }
            if(color === 'green') { return <GreenComponent/>; }
        }


        return (
            <div className="parents">
                { getColoredComponent(color) }
            </div>
        )
    }
})
```


[transform-do-expressions](https://babeljs.io/docs/plugins/transform-do-expressions) 
这个插件就是为了方便在 jsx写if/else表达式而提出的，我们可以重写下代码。

```js
var App = React.createClass({

    render(){
        let { color } = this.props;

        return (
            <div className="parents">
                {do {
                    if(color == 'blue') { 
                        <BlueComponent/>; 
                    }else if(color == 'red') { 
                        <RedComponent/>; 
                    }else { 
                        <GreenComponent/>; }
                    }
                }}
            </div>
        )
    }
})
```

再说说 [transform-function-bind](http://babeljs.io/docs/plugins/transform-function-bind/),
这个插件其实就是提供过 :: 这个操作符来方便快速切换上下文， 如下面的代码：

```js
obj::func
// is equivalent to:
func.bind(obj)

obj::func(val)
// is equivalent to:
func.call(obj, val)

::obj.func(val)
// is equivalent to:
func.call(obj, val)

// 再来一个复杂点的样例

const box = {
  weight: 2,
  getWeight() { return this.weight; },
};

const { getWeight } = box;

console.log(box.getWeight()); // prints '2'

const bigBox = { weight: 10 };
console.log(bigBox::getWeight()); // prints '10'

// Can be chained:
function add(val) { return this + val; }

console.log(bigBox::getWeight()::add(5)); // prints '15'
```


如果想更屌点，还可以写出更牛逼的代码：

```js
const { map, filter } = Array.prototype;

let sslUrls = document.querySelectorAll('a')
                ::map(node => node.href)
                ::filter(href => href.substring(0, 5) === 'https');

console.log(sslUrls);
```

## 包罗万象的stage-1

