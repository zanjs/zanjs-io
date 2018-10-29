---
title: JavaScript实现函数重载
date: 2018-10-29 08:52:28
tags:
	- javascript
	- js函数
categories:
	- javascript	
---



重载是指函数或者方法有相同的名称，但是参数个数或类型不相同的情形，这样的同名不同参的函数或者方法之间，互相称之为重载函数或方法。


我们知道，JavaScript函数可以随意传递任意数量、任意类型的参数，那么它有没有重载呢？


答案是有的，下面我们通过3种方法来实现JavaScript的函数重载。


## 实现

### 目标

我们有一个people对象

```js
var people = {
  values: ['Dean Edwards', 'Sam Stephenson', 'Alex Russell', 'Dean Tom']
};
```

想要实现一个find方法，不传参数的时候，输出所有名字，
只传1个参数的时候，输出所有fristName和参数相同的名字，
传2个参数的时候，输出所有firstName和lastName和2个参数分别相同的名字。

```js
people.find();                          // ["Dean Edwards", "Sam Stephenson", "Alex Russell", "Dean Tom"]
people.find('Dean');                    // ["Dean Edwards", "Dean Tom"]
people.find('Dean', 'Edwards');            // ["Dean Edwards"]
```


- 利用arguments和switch实现重载

```js
people.find = function () {
  switch (arguments.length) {
    case 0:
      return this.values;

    case 1:
      return this.values.filter((value) => {
        var firstName = arguments[0];
        return value.indexOf(firstName) !== -1 ? true : false;
      });

    case 2:
      return this.values.filter((value) => {
        var fullName = `${arguments[0]} ${arguments[1]}`;
        return value.indexOf(fullName) !== -1 ? true : false;
      });
  }
};

console.log(people.find());                 // ["Dean Edwards", "Sam Stephenson", "Alex Russell", "Dean Tom"]
console.log(people.find('Dean'));           // ["Dean Edwards", "Dean Tom"]
console.log(people.find('Dean', 'Edwards'));   // ["Dean Edwards"]
```

这种方式大家肯定都能看懂，就不多说啦。


- 利用arguments和闭包实现重载

```js
function addMethod (object, name, fn) {
  // 把前一次添加的方法存在一个临时变量old中
  var old = object[name];

  // 重写object[name]方法
  object[name] = function () {
    if (fn.length === arguments.length) {
      // 如果调用object[name]方法时，如果实参和形参个数一致，则直接调用
      return fn.apply(this, arguments);
    } else if (typeof old === 'function') {
      // 如果实参形参不一致，判断old是否是函数，如果是，就调用old
      return old.apply(this, arguments);
    }
  };
}

addMethod(people, 'find', function() {
  return this.values;
});

addMethod(people, 'find', function(firstName) {
  return this.values.filter((value) => {
    return value.indexOf(firstName) !== -1 ? true : false;
  });
});

addMethod(people, 'find', function(firstName, lastName) {
  return this.values.filter((value) => {
    var fullName = `${firstName} ${lastName}`;
    return value.indexOf(fullName) !== -1 ? true : false;
  });
});

console.log(people.find());                     // ["Dean Edwards", "Sam Stephenson", "Alex Russell", "Dean Tom"]
console.log(people.find('Dean'));               // ["Dean Edwards", "Dean Tom"]
console.log(people.find('Dean', 'Edwards'));    // ["Dean Edwards"]
```

这里 `addMethod(object, name, fn)` 方法是核心。我们着重分析一下为什么这里会有闭包，可以保存上一个注册的函数


```js
function addMethod (object, name, fn) {
  // object, name, fn是传入的3个参数
  var old = object[name];

  object[name] = function () {
    // 这里对old和fn进行了引用
    if (fn.length === arguments.length) {
      return fn.apply(this, arguments);
    } else if (typeof old === 'function') {
      return old.apply(this, arguments);
    }
  };
}
```


`object` 是另外一个引用对象，它的一个方法中引用了old和fn，所以对于addMethod来说，

它的局部变量在addMethod函数执行完后，仍然被另外的变量所引用，导致它的 执行环境无法销毁，所以产生了闭包 。


因此，每次调用 `addMethod` ，都会有一个执行环境保存着当时的 `old` 和 `fn`，
所以在调用 `people.find()` 的时候可以找到当时注入的fn，实现函数重载。


- 利用Proxy和arguments实现重载

```js
var proxy = new Proxy(people, {
  get: function (target, key, receiver) {
    if (key === 'find') {
      return function () {
        switch (arguments.length) {
          case 0:
            return this.values;
      
          case 1:
            return this.values.filter((value) => {
              var firstName = arguments[0];
              return value.indexOf(firstName) !== -1 ? true : false;
            });
      
          case 2:
            return this.values.filter((value) => {
              var fullName = `${arguments[0]} ${arguments[1]}`;
              return value.indexOf(fullName) !== -1 ? true : false;
            });
        }
      };
    }

    return Reflect.get(target , key , receiver);
  },

  set: function (target, key, value, receiver) {
    return Reflect.set(target, key, value, receiver);
  }
});

console.log(proxy.find());                     // ["Dean Edwards", "Sam Stephenson", "Alex Russell", "Dean Tom"]
console.log(proxy.find('Dean'));               // ["Dean Edwards", "Dean Tom"]
console.log(proxy.find('Dean', 'Edwards'));   // ["Dean Edwards"]
```

这样写其实感觉有点画蛇添足了，就当成是另外一种思路吧。


## 总结

`JavaScript` 可以实现函数重载，主要有两种思想：

1. 利用arguments类数组来判断接收参数的个数
2. 利用闭包保存以前注册进来的同名函数