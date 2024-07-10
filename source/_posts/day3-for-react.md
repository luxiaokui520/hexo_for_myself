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

jsx 和 js 语法都可以实现在 h1 标签中创建 span 标签的功能，但是很明显 jsx 语法更容易让程序员阅读以及书写。
而从下方的 jsx 语法向上方的 js 语法转换只需要 Babel 文件的隐式帮助，因此我们愿意选择 jsx 语法糖来创建虚拟 DOM

### 虚拟 DOM

- 虚拟 DOM 的本质是 **object** 类型的对象（一般对象）
- 虚拟 DOM 比价'**轻**‘，真实 DOM 比较‘**重**’，因为虚拟 DOM 是 react 内部在用，无需真实 DOM 上那么多的标签元素属性
- 虚拟 DOM 最终会被 React 转化为真实 DOM，呈现在页面上

```html
<div id="test"></div>
<div id="demo"></div>
```

```jsx
const VDOM = (
  <h1 id="title">
    <span>Hello, React</span>
  </h1>
);
console.log("虚拟DOM", VDOM); // object
console.log(typeof VDOM); // object
console.log(VDOM instanceof Object); // true

const TDOM = document.getElementById("demo");
console.log("真实DOM", TDOM); // <div id="demo"></div>
```

### JSX 简介

- 全称 javascript XML
- react 定义的一种类似于 XML 的 js 扩展语法：js + XML
- XML 早期用于存储和传输数据，其采用标签的方式进行数据存储
- 由于 json 拥有 parse 和 stringfy 功能，使得数据解析更加便利，因此现在多用 json 进行数据存储和传输

### JSX 语法规则

- 定义虚拟 DOM 时，不要写引号
- 虚拟 DOM 标签中混入 js 表达式时要用`{}`
- 样式的类名指定不要用 class，要用 **className**， 因为 class 是 es6 里面定义的类关键字，因此在 jsx 中要使用 className
- 内联样式，要用 `style={{ key: value }}`的形式去写
  1. 最外层的花括号表示 js 表达式，里面的花括号表示一个对象，即键值对的集合，这样我们就可以理解 value 值是一个字符串了
  2. 一定要将 jsx 语法与 html 标签进行区分，jsx 实际上是 xml 与 js 的结合，而 xml 是一种比 json 更早的使用标签进行数据存储的方式，一定不要单纯的以为 jsx 中的虚拟 DOM 就是 html 标签
- 虚拟 DOM 必须只有一个根标签
- 标签必须闭合
- 虚拟 DOM 标签首字母
  1. 若小写字母开头，则将标签转为 html 中同名元素，若 html 中无该标签对应的同名元素，则报错；
  2. 若大写字母开头，react 就去渲染对应的组件,若组件没有定义，则报错
- 虚拟 DOM 中添加注释的方式
  1. 由于 jsx 中虚拟 DOM 的写法和 html 标签很像，因此有时候会想当然的认为在虚拟 DOM 标签中添加注释的方法与 html 中给标签添加注释的方法相同
  2. 但实际上由于**虚拟 DOM**本质上是**jsx 语法**，也就是 js，所以在虚拟 DOM 中添加注释应该与 js 一致，即通过`//` 或者 `/* */` 来添加注释
  3. 但如果直接用`//` 或者 `/* */`来添加注释会失效，甚至报错，此时发现 jsx 最基本的语法规则是**虚拟 DOM 标签中混入 js 表达式时要用`{}`**
  4. 因此作为 js 语法的`//` 或者 `/* */`被`{}`包裹即可添加注释，又因为`//`作为单行注释，会将整行识别为注释内容，从而无法识别`}`,因此最终采用`{/* */}`的方式来添加注释
  5. 而另外一种方式就是在标签/组件内部直接使用 `<Component /* */ />`来添加注释，属于 jsx 特殊语法范畴

```jsx
const myId = "AtGuiGu";
const myData = "Hello, React";

// 1.创建虚拟DOM
const VDOM = (
  <div>
    <h2 className="title" id={myId.toLowerCase()}>
      <span style={{ color: "white", fontSize: "29px" }}>{myData}</span>
    </h2>
    <h2 className="title" id={myId.toLowerCase() + "2"}>
      <span style={{ color: "white", fontSize: "29px" }}>{myData}</span>
    </h2>
    {/* 组件未定义，会报错 */}
    <Good /* 组件未定义，会报错 */ />
  </div>
); /* 此处一定不要写引号，因为不是字符串 */

// 2.渲染虚拟DOM到页面
ReactDOM.render(VDOM, document.getElementById("test"));
```

### *js 语句（代码）*与*js 表达式*的区别

一定注意区分：**js 语句（代码）**与**js 表达式**

- **表达式**：一个表达式会产生一个值，可以放在任何一个需要值的地方
  下面这些都是表达式：

1. a
2. a + b
3. demo(1)
4. arr.map(() => {})
5. function test() {}

- **语句（代码）**：控制代码走向
  下面这些都是语句（代码）：

1. if(){}
2. for(){}
3. switch() case:

jsx 中虚拟 DOM 用{}包裹的是**js 表达式**，而不是**js 语句**

```jsx
// 模拟一些数据
const data = ["Angular", "React", "Vue"];
// 1.创建虚拟DOM
const VDOM = (
  <div>
    <h1>前端js框架列表</h1>
    <ul>
      {data.map((e, index) => (
        <li key={index}>{e}</li>
      ))}
      {/* 
        <li>Angular</li>
        <li>React</li>
        <li>Vue</li> 
      */}
    </ul>
  </div>
);
```

### 模块

- 向外提供特定功能的 js 程序，一般就是一个 js 文件
- 为什么要拆成模块：随着业务逻辑增加，代码越来越多且复杂
- 作用：复用 js，简化 js 的编写，提高 js 运行效率

### 组件

- 用来实现局部功能效果的代码和资源集合（html/css/js/image/video 等等）
- 原因：一个界面的功能更复杂
- 作用：复用编码，简化项目编码，提高运行效率

### 模块化

当应用的 js 都以模块来编写的，这个应用就是一个模块化的应用

### 组件化

当应用是以多组件的方式来实现，这个应用就是组件化的应用
