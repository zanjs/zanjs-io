title: React 组件之间如何交流
date: 2016-06-11 17:42:23
tags:
  - react
categories:
  - react
---


原著序

处理 React 组件之间的交流方式，主要取决于组件之间的关系，然而这些关系的约定人就是你。

我不会讲太多关于 data-stores、data-adapters 或者 data-helpers 之类的话题。
我下面只专注于 React 组件本身的交流方式的讲解。

React 组件之间交流的方式，可以分为以下 3 种：

- 【父组件】向【子组件】传值；
- 【子组件】向【父组件】传值；
- 没有任何嵌套关系的组件之间传值（PS：比如：兄弟组件之间传值）

## 【父组件】向【子组件】传值

初步使用

这个是相当容易的，在使用 React 开发的过程中经常会使用到，主要是利用 props 来进行交流。例子如下：

```js
// 父组件
var MyContainer = React.createClass({
  getInitialState: function () {
    return {
      checked: true
    };
  },
  render: function() {
    return (
      <ToggleButton text="Toggle me" checked={this.state.checked} />
    );
  }
});

// 子组件
var ToggleButton = React.createClass({
  render: function () {
    // 从【父组件】获取的值
    var checked = this.props.checked,
        text = this.props.text;

    return (
        <label>{text}: <input type="checkbox" checked={checked} /></label>
    );
  }
});
```

进一步讨论

如果组件嵌套层次太深，那么从外到内组件的交流成本就变得很高，通过 props 传递值的优势就不那么明显了。
（PS：所以我建议尽可能的减少组件的层次，就像写 HTML 一样，简单清晰的结构更惹人爱）

```js
// 父组件
var MyContainer = React.createClass({
  render: function() {
    return (
      <Intermediate text="where is my son?" />
    );
  }
});

// 子组件1：中间嵌套的组件
var Intermediate = React.createClass({
  render: function () {
    return (
      <Child text={this.props.text} />
    );
  }
});

// 子组件2：子组件1的子组件
var Child = React.createClass({
  render: function () {
    return (
      <span>{this.props.text}</span>
    );
  }
});
```


## 【子组件】向【父组件】传值


接下来，我们介绍【子组件】控制自己的 state 然后告诉【父组件】的点击状态，
然后在【父组件】中展示出来。
因此，我们添加一个 change 事件来做交互。

```
// 父组件
var MyContainer = React.createClass({
  getInitialState: function () {
    return {
      checked: false
    };
  },
  onChildChanged: function (newState) {
    this.setState({
      checked: newState
    });
  },
  render: function() {
    var isChecked = this.state.checked ? 'yes' : 'no';
    return (
      <div>
        <div>Are you checked: {isChecked}</div>
        <ToggleButton text="Toggle me"
          initialChecked={this.state.checked}
          callbackParent={this.onChildChanged}
          />
      </div>
    );
  }
});

// 子组件
var ToggleButton = React.createClass({
  getInitialState: function () {
    return {
      checked: this.props.initialChecked
    };
  },
  onTextChange: function () {
    var newState = !this.state.checked;
    this.setState({
      checked: newState
    });
    // 这里要注意：setState 是一个异步方法，所以需要操作缓存的当前值
    this.props.callbackParent(newState);
  },
  render: function () {
    // 从【父组件】获取的值
    var text = this.props.text;
    // 组件自身的状态数据
    var checked = this.state.checked;

    return (
        <label>{text}: <input type="checkbox" checked={checked}                 onChange={this.onTextChange} /></label>
    );
  }
});
```

[更多](http://www.tuicool.com/articles/AzQzEbq)