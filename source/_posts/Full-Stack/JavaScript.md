---
title: JavaScript Tutorial
date: 2025-09-01 21:23:14
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/JavaScript.png?raw=true
tags: [JavaScript]
category: Full Stack
category_bar: true
description: Mastering the basic syntax of JavaScript, laying the groundwork for subsequent Node.js backend development!
---

&emsp;&emsp;相信各位读者已经掌握了2~3种编程语言，在此基础上再学习JavaScript并非难事，因此**主要展示大致语法结构**，及JavaScript相比其他语言的“特性”，skip 一些简单的执行流程原理！

## Usage
与在HTML中引入CSS相同，提供两种方式进行引入：

1. 行内式
```html
<script>
    // 这是JavaScript代码
</script>
```

2. 外部式
```html
<script src="js/script.js"></script>
```

{%note info%}
可以将脚本放置在`<head>`或`<body>`中（建议包含在**后者**中）
{%endnote%}

{%note danger%}
外部脚本不能包含`<script>`标签
{%endnote%}

## Basic Syntax
- JavaScript 并不强求每个语句以`;`结束，浏览器中负责执行 JavaScript 代码的引擎会自动在每个语句的结尾补上`;`，但防止某些情况下会改变程序的语义，导致运行结果与期望不一致，我们建议都在结尾使用`;`！

### 注释
```javascript
// 这是单行注释
```

```javascript
/*
这是多行注释
这是多行注释
*/
```

### 输出
#### 控制台

```javascript
console.log("输出一条日志");//最常用
console.info("输出一条信息");
console.warn("输出一条警告");
console.error("输出一条错误");
```
#### 窗口

```javascript
alert("这是一条警告");
```
#### 页面

```javascript
document.write("这是一条警告");
```

### 比较运算符
- 第一种是`==`比较，它会**自动转换数据类型再比较**，很多时候，会得到非常诡异的结果；
- 第二种是`===`比较，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较。

**建议使用`===`比较**，因为它的比较规则更严格，不会出现自动转换数据类型的情况。

{%note danger%}
另一个例外是NaN这个特殊的Number与所有其他值都不相等，包括它自己：
```javascript
NaN === NaN; // false
```

只有当`isNaN()`才相等，返回`true`：

```javascript
isNaN(NaN); // true
```
{%endnote%}

### 基本数据类型
- 数值（Number）
- 字符串（String）
- 布尔值（Boolean）
- `null`
- `undefined`：使用`var`声明变量但未对其加以初始化
- 对象（Object）

与C++等语言不同，JavaScript的数值类型**不区分整数和浮点数**，统一用Number表示，以下都是合法的Number类型：

```javascript
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```

### 声明变量
因为 JavaScript 是**弱类型**语言，因此**变量的类型可以动态改变**。在 JavaScript 中，可以使用`var`、`let`、`const`声明变量，它们的区别是：

- **作用域**不同
   
   - `var` 声明的变量具有**函数作用域**，即只在声明它的函数内部有效，在函数外部无法访问。如果在函数外部使用 `var` 声明变量，则该变量会成为全局变量
   - `let` 和 `const` 声明的变量具有**块级作用域**，即只在声明它的代码块（如 `if`、`for`、`while` 等语句的花括号内）中有效，在代码块外部无法访问

```javascript
function testVar() {
  if (true) {
    var message = "Hello"; // var声明的变量具有函数作用域
    console.log(message); // 输出: Hello
  }
  console.log(message); // 仍能访问，输出: Hello（因为在同一函数内）
}
testVar();
console.log(message); // 报错: message is not defined（函数外部无法访问）
```

```javascript
function testLet() {
  if (true) {
    let message = "Hello"; // let声明的变量具有块级作用域(const同理)
    console.log(message); // 输出: Hello
  }
  console.log(message); // 报错: message is not defined（块外部无法访问）
}
testLet();
```

- **可修改性**不同
   
   - `var` 和 `let` 声明的变量**可以被重新赋值**
   - `const` 声明的变量是**常量**，一旦声明就**不能被重新赋值**。但需要注意的是，如果 `const` 声明的是对象或数组，对象的属性或数组的元素是可以被修改的，因为 `const` 只保证变量**指向的内存地址不变**，而**不保证内存地址中的内容不变**

```javascript
var a = 10;
a = "hello"; // 允许重新赋值
console.log(a); // 输出: hello
```

- **重复声明**不同
    
    - `var` **允许在同一作用域内重复声明同一个变量**
    - `let` 和 `const` 不允许在同一作用域内重复声明同一个变量，否则会抛出 SyntaxError 错误

```javascript
var x = 10;
var x = 20; // 允许重复声明，会覆盖之前的值
console.log(x); // 输出: 20
```

### 循环、控制语句
与 CPP 基本相同，不再赘述

### Function
```javascript
function test1(x) {
    console.log(`Input x value is ${x}`)
    x++;
    let y = 100
    console.log(`Now the x value in this function is ${x}`)
}

let x = 10
test1(x)
console.log(`Now the x outside the function is ${x}`)
```

```text
Input x value is 10
Now the x value in this function is 11
Now the x outside the function is 10
```
- 基本类型的传递为**值传递**
- 对象、数组、函数等类型的传递为**引用传递**

### OOP in JavaScript

## Reference

[【 javascript】用一篇文章让你搞清楚var、let、const声明变量和不用var声明变量的区别](https://blog.csdn.net/weixin_55846296/article/details/126604513?fromshare=blogdetail&sharetype=blogdetail&sharerId=126604513&sharerefer=PC&sharesource=m0_53058983&sharefrom=from_link)