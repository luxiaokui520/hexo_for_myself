---
title: day3 for react
date: 2024-07-04 10:18:37
tags: react
---

# React 的基本使用

### react 依赖包中几个文件的作用

- babel 主要有两种用途：1.用于 es6 向 es5 的转义， 2.用于 react 中 jsx 语法向 js 的转义
- react.development.js react 核心库
- react-dom.development.js react 扩展库,帮助操作 DOM

### react 中相关文件的引用

- 在引入 reactdom 扩展库之前需要优先引入 react 核心库
- 引入 babel 后，使用 jsx 语法时使用的 script 标签类型为`type="text/babel"`
- 在 jsx 语法中，可以将标签赋值给变量，作为可编辑的虚拟 DOM（这也是 jsx 与 js 之间的不同之处）
- 不同的虚拟 DOM 不能叠加在相同 id 的容器内，只会渲染最新的虚拟 DOM 内容，如果想要效果叠加，需要引入组件的概念

```html
<body>
  <!-- 准备好一个容器 -->
  <div id="test"></div>

  <!-- 引入React核心库 -->
  <script type="text/javascript" src="../js/react.development.js"></script>

  <!-- 引入react-dom， 用于支持react操作dom -->
  <script type="text/javascript" src="../js/react-dom.development.js"></script>

  <!-- 引入Babel，用于将jsx转化为js -->
  <script type="text/javascript" src="../js/babel.min.js"></script>

  <script type="text/babel">
    /* 此处一定要写babel*/

    // 1.创建虚拟DOM
    const VDOM = <h1>Hello, React</h1>; /* 此处一定不要写引号，因为不是字符串 */

    // 2.渲染虚拟DOM到页面
    ReactDOM.render(VDOM, document.getElementById("test"));
  </script>
</body>
```

### 为什么我们需要使用 jsx 语法

在使用 react 框架进行页面渲染时，一定要经历的两个步骤就是创建虚拟 DOM 了，以及渲染虚拟 DOM 到页面。
在创建虚拟 DOM 的时候一共有两种方式，分别是 js 和 jsx，接下来可以根据例子来看看两者的区别

_使用 js 创建虚拟 DOM_

```html
<script type="text/javascript">
  // 1.创建虚拟DOM
  const VDOM = React.createElement(
    "h1",
    { id: "title" },
    React.createElement("span", {}, "Hello, React")
  );
  // 2.渲染虚拟DOM到页面
  ReactDOM.render(VDOM, document.getElementById("test"));
</script>
```

_使用 jsx 创建虚拟 DOM_

```html
<script type="text/babel">
  /* 此处一定要写babel*/

  // 1.创建虚拟DOM
  const VDOM = (
    <h1 id="title">
      <span>Hello, React</span>
    </h1>
  ); /* 此处一定不要写引号，因为不是字符串 */

  // 2.渲染虚拟DOM到页面
  ReactDOM.render(VDOM, document.getElementById("test"));
</script>
```

jsx 和 js 语法都可以实现在 h1 标签中创建 span 标签的功能，但是很明显 jsx 语法更容易让程序员阅读以及书写
而从下方的 jsx 语法向上方的 js 语法转换只需要 Babel 文件的隐式帮助，因此我们愿意选择 jsx 语法糖来创建虚拟 DOM

### 虚拟 DOM

- 虚拟 DOM 的本质是 object 类型的对象（一般对象）
- 虚拟 DOM 比价'轻‘，真实 DOM 比较‘重’，因为虚拟 DOM 是 react 内部在用，无需真实 DOM 上那么多的属性
- 虚拟 DOM 最终会被 React 转化为真实 DOM，呈现在页面上
