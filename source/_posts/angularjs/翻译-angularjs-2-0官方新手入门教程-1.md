title: ' 翻译 angularjs 2.0官方新手入门教程(1) '
date: 2016-02-07 12:28:36
tags:
  - angularjs
  - javascript
categories:
  - angularjs
---

## 前言：

前些日子在公司同事的提议下接触了一下angular 2.0，跟着angularjs 2.0官方网站的上手教程做了一遍，
感觉这教程还是不错的，步骤清晰且没有冗余的内容，非常适合新手入门。
但是后来在网上看见不止一个人说跟着官方教程跑都跑不起来，所以怀疑是不是他们英语阅读有问题，
就打算把原教程实现的步骤翻译过来，并在过程中提供一些注意事项。
这次翻译的教程是一个五分钟的hello world教程，官网后面还有一些复杂一点的，以后有空再陆续翻译。
当然，我还是强烈建议去阅读官方原教程，因为里面除了实现以外对每一步都有非常详尽的解释，这是我这次翻译做不到的。
不过如果有问题也可以在评论里留言，我会尽量解答我能解答的，当然也欢迎指出里面毛病。


## 准备：


做前端并不需要你去下载和配置很多东西，甚至angularjs 2.0的库文件都不需要你亲手去下载（事实上，你在他们官网也找不到下载的地方）。
你需要做的只是找个代码编辑器，把npm装好，写好包依赖的json文件，之后npm会自动帮你把需要的东西下载下来（这也是目前前端领域的惯用做法，具体怎么做后面会说）。

所以，要完成这个教程你第一步要干的是弄好以下两样东西：


- npm，官网教程：https://docs.npmjs.com/getting-started/installing-node。注意：安装过程中碰到npm WARN无视就好，
 如果在安装的最后碰到npm ERR则表明失败，失败的原因可能是npm没装好，也可能是因为伟大的GFW，所以你可能需要翻墙，请务必自备VPN。
 （多说一句，不翻墙，就别写代码，因为你解决问题的效率将是翻墙的十分之一都不到。）

- 一个顺手的代码编辑器，比如微软的visual studio code，或者sublime，或者随便什么。

-  Chrome，做前端调试网页必备浏览器。




如果配置npm出问题，请到谷歌或者stackoverflow（不要用百度）搜索命令行里提示给你们的错误信息，去找解决方法。


以上三项准备好了以后，可以正式开始写代码了。


### 第一步：写包依赖json文件，并下载所有的包




- 随便找个什么地方，建一个文件夹，文件夹名叫angular2-quickstart

- 在angular2-quickstart文件夹里建一个文件叫package.json

- 复制以下内容到package.json

```
{
  "name": "angular2-quickstart",
  "version": "1.0.0",
  "scripts": {
    "tsc": "tsc",
    "tsc:w": "tsc -w",
    "lite": "lite-server",
    "start": "concurrent \"npm run tsc:w\" \"npm run lite\" "
  },
  "license": "ISC",
  "dependencies": {
    "angular2": "2.0.0-beta.3",
    "systemjs": "0.19.6",
    "es6-promise": "^3.0.2",
    "es6-shim": "^0.33.3",
    "reflect-metadata": "0.1.2",
    "rxjs": "5.0.0-beta.0",
    "zone.js": "0.5.11"
  },
  "devDependencies": {
    "concurrently": "^1.0.0",
    "lite-server": "^2.0.1",
    "typescript": "^1.7.5"
  }
}
```



>打开命令行，angular2-quickstart目录下输入 `npm install`，然后等着就行了（此步骤如果失败可能需要翻墙）。


#### 配置Typescript（以下简称ts）


首先说一下，Typescript是一门语言，是JavaScript（以下简称js）的超集，
至于为什么要用这个语言，问angularjs的官网吧（官网提供了三种语言版本的教程：js/ts/dart。其中只有ts这个语言的教程是比较全的，
不仅包含五分钟上手教程，还包含了后面比较复杂的教程）。好消息是这个语言看着比js还要简洁，而且很容易上手。

好，那么配置他也是很简单的事情，你需要做的事情和(2)是类似的，
首先在angular2-quickstart文件夹里建一个文件叫 `tsconfig.json`，并复制以下内容到 `tsconfig.json`里就可以了。


```
{
  "compilerOptions": {
    "target": "es5",
    "module": "system",
    "moduleResolution": "node",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  },
  "exclude": [
    "node_modules"
  ]
}
```


### 第二步：写ts源码



- 在angular2-quickstart目录下建一个文件夹app，并在app文件夹里建一个文件叫app.component.ts

- 复制以下内容到app.component.ts


```
import {Component} from 'angular2/core';

@Component({
    selector: 'my-app',
    template: '<h1>My First Angular 2 App</h1>'
})
export class AppComponent { }
```


简单说一下，angularjs 2.0这个框架走的是组件化路线（前端大趋势），所以其实这里我们是建立了一个可视化的组件，
我们可以看出来一个组件大概是由两部分组成的，一个是@Component，在我们这个教程里，Component定义了他在html上的标签名以及html代码（他是什么样子），
当然Component里还有一些其他的功能，比如定义CSS和route等等。而另一部分是class，里面定义了他的业务逻辑（他要干什么），
但目前我们也不需要在里面写什么东西，毕竟只是个hello world。


这里我们可以看到他的class前面有个export关键词，根据官方的解释，
export这个动作把这个ts文件变成了一个组件，这样一来别的地方就可以import并使用他了，
就像第一行里我们 import {Component} from 'angular2/core' 一样。


-  在app目录下建一个文件叫main.ts

- 复制以下内容到main.ts

```
import {bootstrap}    from 'angular2/platform/browser'
import {AppComponent} from './app.component'

bootstrap(AppComponent);
```


看，我们刚才写的AppComponent就被import到这个main.ts里了。而整个网页代码的入口也就是AppComponent。



### 第三步：写html

- 在angular2-quickstart目录下建一个html文件叫index.html

- 复制以下内容到index.html

```
<html>
  <head>
    <title>Angular 2 QuickStart</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">    

    <!-- 1. Load libraries -->
    <!-- IE required polyfills, in this exact order -->
    <script src="node_modules/es6-shim/es6-shim.min.js"></script>
    <script src="node_modules/systemjs/dist/system-polyfills.js"></script>

    <script src="node_modules/angular2/bundles/angular2-polyfills.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <script src="node_modules/rxjs/bundles/Rx.js"></script>
    <script src="node_modules/angular2/bundles/angular2.dev.js"></script>

    <!-- 2. Configure SystemJS -->
    <script>
      System.config({
        packages: {        
          app: {
            format: 'register',
            defaultExtension: 'js'
          }
        }
      });
      System.import('app/main')
            .then(null, console.error.bind(console));
    </script>
  </head>

  <!-- 3. Display the application -->
  <body>
    <my-app>Loading...</my-app>
  </body>

</html>
```

--- 


我们可以看出来，这个index.html分为三块。第一块引入了必要的js文件，他们都存在npm下载好的目录下。
第二块配置了 `SystemJS`，并在里面 `import` 了我们刚才写好的 `main.ts`，`main.ts`里提供了代码的入口即`bootstrap`。
第三块就是显示出来我们最开始写的app.component.ts，你应该还有印象，在 `app.component.ts` 的代码里有一句是 `selector: 'my-app'`，
所以在html里这个组件就用 `<my-app></my-app>`表示，
这个标签显示的内容就是  ` template: '<h1>My First Angular 2 App</h1>' ` 中的  `<h1>My First Angular 2 App</h1>'`，
而那个Loading...则会显示在js文件加载完之前，加载之后就会被template里的内容替代。


### 第四步：编译运行

很简单就一步，`angular2-quickstart` 目录下在命令行里输入 `npm start`，之后不要关闭命令行，等待一会儿会有个网页自动打开就可以看到结果了。


