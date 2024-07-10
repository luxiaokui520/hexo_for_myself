---
title: day4 for react
date: 2024-07-08 19:32:45
tags: react
---

# React 面向组件编程

### 函数式组件

```jsx
function Demo() {
  console.log(this); // undefined(babel编译为es5开启了严格模式‘use strict’， this不能指向window)
  return <h2>我是用函数定义的组件（适用于【简单组件】的定义）</h2>;
}

// 2.渲染组件到页面
ReactDOM.render(<Demo />, document.getElementById("test"));
// ReactDOM.render(Demo(), document.getElementById("test"));
```

执行了 `ReactDOM.render(<Demo/>...)`之后，发生了什么？

- React 解析组件标签，找到了 `Demo` 组件
- 发现组件是由函数定义的，随后调用该函数，将返回的虚拟 DOM 转为真实 DOM，随后呈现在页面中
- 如果用了 `Demo()`，是自己在调用，这个时候 Demo 的大小写无所谓
- 但是使用`<Demo/>`就是 React 在调用，这个时候定义的 Demo 函数组件一定要大写，才能被 React 作为组件标签进行解析

### js 类

- 类中的构造器不是必须的，如果要对实例进行一些初始化的操作，如添加指定属性时才写
- 如果 A 类继承了 B 类，且 A 类中写了构造器，那么 A 类构造器中的 `super` 是必须要调用的（且要在最开始进行调用）
- 类中所定义的方法，都是放在了类的**原型对象**上，供实例使用
- 构造器中的 this 是谁？ - 类的实例对象
- 类里定义的方法放在了哪里? - 类的原型对象上，供实例使用
- 通过实例调用类里的方法时，方法中的 this 就是实例（不包括 call，apply 等可以更改 this 指向的调用方法）

```js
// 创建一个Person类
class Person {
  // 构造器方法
  constructor(name, age) {
    // 构造器中的this是谁？ - 类的实例对象
    this.name = name;
    this.age = age;
  }
  // 一般方法
  speak() {
    // speak方法放在了哪里? - 类的原型对象，供实例使用
    // 通过Person实例调用speak时，speak中的this就是Person实例
    console.log(`我叫${this.name}, 我年龄是${this.age}`);
  }
}

// 创建一个Person的实例对象
const p1 = new Person("Json", 18);
const p2 = new Person("Jack", 20);
p1.speak(); // 我叫Json, 我年龄是18
p2.speak(); // 我叫Jack, 我年龄是20

console.log(p1); // Person {name: 'Json', age: 18}
console.log(p2); // Person {name: 'Jack', age: 20}

// 创建一个Student类，继承Person类
class Student extends Person {
  constructor(name, age, grade) {
    super(name, age); // 必须在最开始调用super，继承父类属性
    this.grade = grade;
  }

  // 重写从父类继承过来的speak方法
  speak() {
    console.log(
      `我叫${this.name}, 我年龄是${this.age}， 我的年纪是${this.grade}`
    );
  }

  // study方法放在了哪里? - 类的原型对象，供实例使用
  // 通过Student实例调用study时，study中的this就是Student实例
  study() {
    console.log("我很努力的学习");
  }
}
const s1 = new Student("小张", 15, "高一");
s1.speak(); // 我叫小张, 我年龄是15， 我的年纪是高一
s1.study(); // 我很努力的学习

console.log(s1); // Student {name: '小张', age: 15, grade: '高一'}
```

### 原型链

上面的 js 代码中 s1 作为 Student 的实例，拥有了 Student 中定义的属性，同时可以使用 Student 中定义的方法
在输出 s1 时，可以看到一个**proto**属性，展开之后是 Student 的原型对象，里面有 Student 中定义的 speak()、study()方法
因此我们可以得到一个结论，即`s1.__proto__ === Student.prototype`
而 Student 继承 Person 类，因此

```js
console.log(s1.__proto__ === Student.prototype); // true
```

### 类式组件

执行了`ReactDOM.render(<MyComponent />,....)`之后，发生了什么？

- React 解析组件标签，找到了 `MyComponent` 组件
- 发现组件是由类定义的，随后 `new` 出来该类（`MyComponent`）的实例，并通过该实例调用原型上的 `render` 方法
- 将 render 返回的虚拟 DOM 转为真实 DOM， 随后呈现在页面上

```jsx
// 1。创建类式组件
class MyComponent extends React.Component {
  render() {
    // render放在MyComponent的原型对象上，供实例使用
    // render中的this是谁？ - MyComponent（组件）的实例对象
    console.log("render中的this：", this); // MyComponent{...} 其中props，state，refs是组件最重要的三个属性
    return <h2>我是用类定义的组件（适用于【复杂组件】的定义）</h2>;
  }
}

//  2.渲染组件到页面
ReactDOM.render(<MyComponent />, document.getElementById("test"));
```

### 复杂组件与简单组件

- 复杂组件存在三大属性，即 `props`，`state`，`refs`
- 之所以说类式组件能够定义复杂组件，是因为`React.Component`中定义了这三大属性，组件实例继承了这三大属性，因此创造出来的组件为复杂组件。
- 目前学习的函数组件中还无法定义这三大属性，因此函数只能定义简单组件
- 以后的学习中，会认识 `hooks`，这将使得函数也能够定义具有三大属性的复杂组件
