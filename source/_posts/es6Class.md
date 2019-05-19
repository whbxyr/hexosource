---
title: es6(二) 之用 es5 理解 es6 的 class
date: 2019-05-13 23:43:37
categories: [技术类-前端]
tags: [JavaScript, es6]
---
&emsp;&emsp;es6 新特性 class 能够像其他面向对象的语言那样创建类并实现类之间的继承，然而实际上底层的实现依然是基于原型链的，因此完全可以将 es6 的 class 翻译成 es5 的原生 function 来理解。

#### <font color="#f00">声明一个类</font>
```javascript
// class 实现
class A {}

// function 实现
function A() {}
```

#### <font color="#f00">声明一个类并定义其原型方法 foo()</font>
```javascript
// class 实现
class A {
  foo() {}
}

// function 实现
function A() {}
A.prototype.foo = function () {}
```

#### <font color="#f00">声明两个类 A 和 B，其中 B 继承于 A，包括原型上的继承以及静态属性、静态方法的继承</font>
```javascript
// class 实现
class A {}
class B extends A {}

// function 实现
function A {}
function B {}
Object.setPrototypeOf(B, A) // 等同于 B.__proto__ = A
Object.setPrototypeOf(B.prototype, A.prototype) // 等同于 B.prototype.__proto__ = A.prototype
```