layout: 如何百倍加速
title: Lo-Dash？引入惰性计算
date: 2016-01-23 17:36:42
tags:
    - 数组
    - JavaScript
    - lodash
categories:
	- javascript
---


我一直以为像 Lo-Dash 这样的库已经不能再快了，毕竟它们已经足够快了。
 Lo-Dash 几乎完全混合了各种 JavaScript 奇技淫巧（YouTube）来压榨出最好的性能。
 
 ## 惰性计算
 
 但似乎我错了 - 其实 Lo-Dash 可以运行的更快。 
 你需要做的是，停止思考那些细微的优化，并开始找出更加适用的算法。 
 例如，在一个典型的循环中，我们往往倾向于去优化单次迭代的时间
 
```js
    var len = getLength();
    for(var i = 0; i < len; i++) {
        operation(); // <- 10毫秒 - 如何优化到9毫秒?!
    }

```

代码说明：取得数组的长度，然后重复执行 N 遍 operation() 函数。译注 by @justjavac

但是，这（优化 operation() 执行时间）往往很难，而且对性能提升也非常有限。

 相反，在某些情况下，我们可以优化 getLength() 函数。 它返回的数字越小，则每个 10 毫秒循环的执行次数就越少。

这就是 Lo-Dash 使用惰性计算的思想。 这是减少周期数，而不是减少每个周期的执行时间。 让我们看看下面的例子：



```js
function priceLt(x) {
   return function(item) { return item.price < x; };
}
var gems = [
   { name: 'Sunstone', price: 4  },
   { name: 'Amethyst', price: 15 },
   { name: 'Prehnite', price: 20 },
   { name: 'Sugilite', price: 7  },
   { name: 'Diopside', price: 3  }, 
   { name: 'Feldspar', price: 13 },
   { name: 'Dioptase', price: 2  }, 
   { name: 'Sapphire', price: 20 }
];

var chosen = _(gems).filter(priceLt(10)).take(3).value();
```

代码说明：gems 保存了 8 个对象，名字和价格。priceLt(x) 函数返回价格低于 x 的所有元素。译注 by @justjavac

我们把价格低于 10 美元的前 3 个 gems 找出来。 常规 Lo-Dash 方法（严格计算）是过滤所有 8 个 gems，然后返回过滤结果的前 3 个。


![](http://justjavac.com/assets/images/lodash-naive.gif)



不难看出来，这种算法是不明智的。 它处理了所有的 8 个元素，而实际上我们只需要读取其中的 5 个元素就能得到我们想要的结果。
 与此相反，使用惰性计算算法，只需要处理能得到结果的最少数量就可以了。 如图所示：
 
 
 ![](http://justjavac.com/assets/images/grafika.gif)
 
 
 
 我们轻而易举就获得了 37.5％ 的性能提升。
  但是这还不是全部，其实很容易找到能获得 1000 倍以上性能提升的例子。 让我们一起来看看：
  
  
  ```js
  // 99,999 张照片
var phoneNumbers = [5554445555, 1424445656, 5554443333, … ×99,999];

// 返回包含 "55" 的照片
function contains55(str) {
    return str.contains("55"); 
};

// 取 100 张包含 "55" 的照片
var r = _(phoneNumbers).map(String).filter(contains55).take(100);
  ```