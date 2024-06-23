---
title: day1 for js review
date: 2024-06-12 14:56:49
tags: JS
---

## Js 组成部分

- ECMAScript
- BOM 整个浏览器
- DOM document 文档,从&lt;html>开始到&lt;html>结束

## js 书写在 html 中的常见规则

- `type = "text/javascript"` 说明当前 script 标签中文本的类型

```html
<script type="text/javascript">
  alert("HELLO WORLD!"); // 在页面上弹出警告框
  document.write("HELLO WORLD!"); // 在当前文本上输入内容
</script>
```

- 所有的 JS 代码都必须写在 script 标签里
- 可以引入多个 script 标签，多个 script 标签之间顺序执行。
- 为了语法规范，script 标签写在 head 标签中
- js 代码可以外部引入，src 引入外部文件

```html
<script type="text/javascript" src="demo.js">
  alert(1); // 这里代码执行不到
</script>
```

- 如果当前 script 标签作用引入外部文件，这个 script 标签中，就不能再写代码了。

## js 变量

- 常量/字面量 确定的值叫做常量
- js 中的数据类型分为两大类
- 基本数据类型

1. 数字 `number` 100 3.14
2. 字符串 `string` 所有带双引号/单引号 `'hello' "hello"`
3. 布尔值 `boolean` `true` `false`
4. 特殊数据类型 `null`空 `undefined`未声明

- 复合数据类型
- 变量，值可以改变的叫做变量
- 声明变量的同时给变量进行赋值，这个过程叫做初始化

1. 声明变量，通过关键字（系统定义的有特殊功能的单词）var 进行声明
2. 变量赋值
3. 可以同时定义多个变量，变量之间要使用逗号进行隔开
4. 标识符：用户自定义的所有名字都叫做标识符、变量名

- 规律：

1. 标识符必须由数字、字母、下划线和美元符号$组成
2. 不能以数字开头
3. 标识符区分大小写，age 和 Age 是两个变量
4. 标识符必须见名思意

```html
<script>
  var age = 18;
  age = 20;
  alert("age:" + age); // age:20
  var name = "xiaoxian",
    age = 18,
    sex = "女";
  alert(sex); // 女
</script>
```

### 其他

- 输出当前变量/常量的数据类型
- 格式为 typeof 变量/常量
- JS 是弱语言，变量被赋值成什么类型就是什么类型，不要在后续的代码里改变该变量的数据类型，很容易引起该代码的歧义

```html
<script>
  var name = "xiaoxian";
  alert("name的数据类型:" + typeof name); // string
  name = 18;
  alert("name的数据类型:" + typeof name); // string

  var age = 18;
  alert("name的数据类型:" + typeof age); // number
  age = "xiaoxian";
  alert("name的数据类型:" + typeof name); // string
</script>
```
