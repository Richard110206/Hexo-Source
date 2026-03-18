---

title: JavaScript Tutorial
date: 2025-09-01 21:23:14
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/JavaScript.png?raw=true
tags: [JavaScript]
category: Full Stack
category_bar: true
description: Mastering the basic syntax of JavaScript, laying the groundwork for subsequent Node.js backend development!
---

&emsp;&emsp;相信各位读者已经掌握了2~3种编程语言，在此基础上再学习`JavaScript`并非难事，因此**主要展示大致语法结构**，及JavaScript相比其他语言的“特性”，skip 一些简单的执行流程原理！

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

```javascript
let username;
username=window.prompt("What's your username?");

console.log(username)  
```

```javascript
let username;
document.getElementById("mySubmit").onclick=function(){
// 函数中是我们点击mySubmit标签ID的button后会触发的行为
username=document.getElementById("myText").value;
console.log(username);
document.getElementById("myH1").textContent=`Hello ${username}`;
}
```

- 使用双引号不能嵌入变量需要使用`+`，反引号支持`${}`直接嵌入变量

```javascript
const username = "hello";
const age = 20;

// 双引号字符串：拼接变量需要用 + 号
const str1 = "Hello " + username + "，你的年龄是 " + (age + 1) + " 岁";
console.log(str1);

// 模板字符串：直接用 ${} 嵌入变量/表达式
const str2 = `Hello ${username}，你的年龄是 ${age + 1} 岁`;
console.log(str2); 
```



### 比较运算符

- 第一种是`==`比较，它会**自动转换数据类型再比较**，很多时候，会得到非常诡异的结果；
- 第二种是`===`比较，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较。

**建议使用`===`比较**，因为它的比较规则更严格，不会出现自动转换数据类型的情况。

{%note danger%}
另一个例外是`NaN`这个特殊的Number与所有其他值都不相等，包括它自己：

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

```javascript
let age=window.prompt("How old are you?");
age+=1; // 进行字符串的拼接
console.log(age,typeof age);
age=Number(age);
age+=1; 
console.log(age,typeof age);
```

- 数据类型转换

```javascript
let x="pizza";
let y="pizza";
let z="pizza";

x=Number(x);
y=String(y);
z=Boolean(z);

console.log(x,typeof x) // NaN 'number'
console.log(y,typeof y) // pizza string
console.log(z,typeof z) // true 'boolean' 空字符串输出false  
```

- `Math`

```javascript
z=Math.round(x); // 四舍五入
z=Math.floor(x);
z=Math.ceil(x);
z=Math.trunc(x); // 直接截断数字 x 的小数部分
z=Math.pow(x,y);
z=Math.sin(x);
z=Math.con(x);
z=Math.tan(x);
z=Math.abs(x);
z=Math.sign(x); // >0得1；<0得-1；=0得0
let max=Math.max(x,y,z);
let min=Math.min(x,y,z);
```

- `String Slice`

```javascript
const fullname="Qinxuan Li";

let firstname=fullname.slice(0,7);
let lastname=fullname.slice(8,10);
console.log(firstname); // Qinxuan
console.log(lastname); // Li 

let lastchar=fullname.slice(-1);
console.log(lastchar); // i
```

```javascript
const email="richard110206@gmail.com"

 let username=email.slice(0,email.indexOf("@"));
 let extension=email.slice(email.indexOf("@")+1);
 console.log(username); // Qinxuan
 console.log(extension); // Li 
```

- `Method Chaining`

```javascript
let username=window.prompt("Enter your usernamse:");
//----------------Normal Method----------------//
username=username.trim();
let letter=username.charAt(0);
letter=letter.toUpperCase();
let extra=username.slice(1);
extra=extra.toLowerCase();
username=letter+extra;
console.log(username); // Qinxuanli
//----------------Method Chaining---------------//
username=username.trim().charAt(0).toUpperCase()+username.trim().slice(1).toLowerCase();
console.log(username); // Qinxuanli
```



### 声明变量

因为 JavaScript 是**弱类型**语言，因此**变量的类型可以动态改变**。在 JavaScript 中，可以使用`var`、`let`、`const`声明变量，它们的区别是：[【 javascript】用一篇文章让你搞清楚var、let、const声明变量和不用var声明变量的区别](https://blog.csdn.net/weixin_55846296/article/details/126604513?fromshare=blogdetail&sharetype=blogdetail&sharerId=126604513&sharerefer=PC&sharesource=m0_53058983&sharefrom=from_link)

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

###  Projects

#### (i).Counter Program

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <label id="countLabel">0</label><br>

<div id="btnContainer">
    <button id="decreaseBtn" class="buttons">decrease</button>
    <button id="resetBtn" class="buttons">reset</button>
    <button id="increaseBtn" class="buttons">increase</button>
</div>
    <script src="index.js"></script>
</body>
</html>
```

```javascript
const decreaseBtn=document.getElementById("decreaseBtn");
const resetBtn=document.getElementById("resetBtn");
const increaseBtn=document.getElementById("increaseBtn");
const countLabel=document.getElementById("countLabel")

let count=0;
increaseBtn.onclick=function(){
    count++;
    countLabel.textContent=count;
}

decreaseBtn.onclick=function(){
    count--;
    countLabel.textContent=count;
}

resetBtn.onclick=function(){
    count=0;
    countLabel.textContent=count;
}
```

```css
#countLabel{
    display:block;
    text-align:center;
    font-size: 10em;
    font-family:Helvetica;
}

#btnContainer{  /* id选择器 */
    text-align:center;
}

.buttons{ /* 类选择器 */
    padding:10px,20px;
    font-size:1.5em;
    color:white;
    background-color: rgb(127, 86, 199);
    border-radius:5px;
    cursor:pointer; /* 悬停时变为手指 */
    transition:background-color 0.25s; /* 按钮背景颜色的过渡 */
}

.buttons:hover{
    background-color:rgb(181, 131, 227);
}
```

#### (ii).Random Number

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <button id="myButton">roll</button><br>
    <label id="label1" class="myLabels"></label><br>
    <label id="label2" class="myLabels"></label><br>
    <label id="label3" class="myLabels"></label><br>
    <script src="index.js"></script>
</body>
</html>
```

```javascript
const myButton=document.getElementById("myButton");
const label1=document.getElementById("label1")
const label2=document.getElementById("label2")
const label3=document.getElementById("label3")
const min=1;
const max=6;
let randomNum1;
let randomNum2;
let randomNum3;


myButton.onclick=function(){
    randomNum1=Math.floor(Math.random()*max)+min;
    randomNum2=Math.floor(Math.random()*max)+min;
    randomNum3=Math.floor(Math.random()*max)+min;
    label1.textContent=randomNum1;
    label2.textContent=randomNum2;
    label3.textContent=randomNum3;
}
```

```css
body{
    font-family:Verdana;
    text-align:center;
}

#myButton{
    font-size:3em;
    padding:20px 25px;
    border-radius:5px;
}

.myLabels{
    font-size:3em;
}
```

#### (iii).CheckBox

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <input type="checkbox" id="myCheckBox">
    <label for="myCheckBox">subscribe</label><br><br>

    <input type="radio" id="visaBtn" name="card">
    <label for="visaBtn">Visa</label><br>
    <input type="radio" id="masterCardBtn" name="card">
    <label for="masterCardBtn">MasterCard</label><br>
    <input type="radio" id="payPalBtn" name="card">
    <label for="payPalBtn">PayPal</label><br>

    <button type="submit" id="mySubmit">submit</button>
    
    <p id="subResult"></p>
    <p id="submitResult"></p>
    <p id="paymentResult"></p>
    <script src="index.js"></script><br>
</body>
</html>
```

- `for`是`<label>`标签特有的属性，作用是**将标签与对应的表单元素绑定**，值必须等于目标表单元素的`id`值。
  - **提升用户体验**：点击`<label>`标签时，会自动触发对应的复选框 / 单选按钮的选中状态，无需精准点击按钮本身

```javascript
const myCheckBox=document.getElementById("myCheckBox");
const visaBtn=document.getElementById("visaBtn");
const masterCardBtn=document.getElementById("masterCardBtn");
const payPalBtn=document.getElementById("payPalBtn");
const mySubmit=document.getElementById("mySubmit");
const subResult=document.getElementById("subResult");
const paymentResult=document.getElementById("paymentResult")

mySubmit.onclick=function(){
    if(myCheckBox.checked){
        subResult.textContent="You are subscribed!"
    }
    else {
        subResult.textContent="You are NOT subscribed!"
    }
    if(visaBtn.checked){
        paymentResult.textContent="You are paying with visa!"
    }
    else if(masterCardBtn.checked){
        paymentResult.textContent="You are paying with masterCard!"
    }
    else if(payPalBtn.checked){
        paymentResult.textContent="You are paying with paypal!"
    }
    else {
        paymentResult.textContent="You must select a payment type!"
    }
}
```

```css
body{
    font-family:Verdana;
    font-size:2em;
}
#mySubmit{
    font-size:1em;
}
```

