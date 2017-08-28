---
title: es6(一) 之 let 和 const 命令
date: 2016-12-26 14:27:31
categories: [技术类-前端]
tags: [JavaScript, es6]
---
### <font style="color: #f00;">（一）let命令以及块级作用域</font>
(1)  let命令声明的变量只在所在的`代码块内`有效
(2)  for循环的循环语句是一个`父级作用域`，而循环体内部是一个单独的`子作用域`
(3)  for循环的每次循环中的变量`i`都是`重新声明`的，作用域之间无关系
(4)  `不存在声明提前`，在声明之前使用变量会报错“ReferenceError: i is not defined”
(5) 存在`暂时性死区`，只要块级作用域内存在let命令，它所声明的变量就”绑定“（binding）这个区域，不再受外部的影响。
+ 因为`“暂时性死区”`的存在，使得`typeof`不再是一个百分之百安全的操作
```javascript
typeof x; // ReferenceError
let x; // 在x声明之前，都属于x的死区，只要用到该变量就会报错
```
(6) `不允许重复声明`。`let`不允许在相同作用域内，重复声明同一个变量。`let`、`var`以及`const`三者是不允许在相同作用域内声明同一个变量的。
```javascript
// 报错
function () {
  let a = 10;
  var a = 1;
}

// 报错
function () {
  let a = 10;
  let a = 1;
}

function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}
```
(7) `ES6 块级作用域`的实现实际上使得广泛应用的立即执行函数表达式（IIFE）不再必要了。
```javascript
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```
(8) `块级作用域与函数声明`。
+ ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域中声明。但是浏览器为了兼容以前的旧代码，还是支持在块级作用域之中声明函数。
+ ES6 引入了块级作用域，明确允许在块级作用域中声明函数，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。但是为了减轻因此产生的不兼容问题，ES6 在`附录B`中规定：
	1. 允许在块级作用域内声明函数。
	2. 函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部。
	3. 同时，函数声明还会提升到所在的块级作用域的头部。
+ 以上三条规则只对 ES6 的浏览器实现有效，其他环境的实现不用遵守，还是将块级作用域的函数声明当左`let`处理。
```javascript
// 浏览器的 ES6 环境
function f() {
  console.log('I am outside!');
}

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() {
      console.log('I am inside!');
    }
  }
  f(); // Uncaught TypeError: f is not a function
}())

// 相当于
(function () {
  var f = undefined;
  if (false) {
    // 重复声明一次函数f
    function f() {
      console.log('I am inside!');
    }
  }
  f(); // Uncaught TypeError: f is not a function
}())
```
(9) 另外，还有一个需要注意的地方。ES6 的块级作用域允许声明函数的规则，只在使用大括号的情况下成立，如果没有使用大括号，就会报错。
```javascript
// 不报错
'use strict';
if (true) {
  function f() {}
}

// 报错
'use strict';
if (true)
  function f() {}
```

### <font style="color: #f00;">（二）const命令</font>
(1) const 命令除了不适用于循环之外，拥有let 命令的所有特性。
(2) const `本质上`并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。
```javascript
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only

const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错

// 如果真的想将对象冻结，应该使用Object.freeze方法。
const foo = Object.freeze({});
// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```
(3) 以下展示的是将一个对象彻底冻结的函数。
```javascript
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```