---
title: es6(二) 之用 es5 理解 es6 的 class
date: 2019-05-13 23:43:37
categories: [技术类-前端]
tags: [JavaScript, es6]
---
&emsp;&emsp;es6 新特性 class 能够像其他面向对象的语言那样创建类并实现类之间的继承，然而实际上底层的实现依然是基于原型链的，因此完全可以将 es6 的 class 翻译成 es5 的原生 function 来理解。
```javascript
// 用 class 声明一个类
class A {}
// 用 function 声明一个类
function A() {}

// 用 class 声明一个类并定义其原型方法 foo()
class A {
  foo() {}
}
// 用 function 声明一个类并定义其原型方法 foo()
function A() {}
A.prototype.foo = function () {}
```