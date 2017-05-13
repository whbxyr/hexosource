---
title: es6(一) 之 let 和 const 命令
date: 2016-12-26 14:27:31
categories: [技术类-前端]
tags: [JavaScript, es6]
---
### <font style="color: #f00;">let命令</font>
```javascript
```
(1)  let命令声明的变量只在所在的`代码块内`有效
(2)  for循环的循环语句是一个`父级作用域`，而循环体内部是一个单独的`子作用域`
(3)  for循环的每次循环中的变量`i`都是`重新声明`的，作用域之间无关系
(4)  `不存在声明提前`，在声明之前使用变量会报错“ReferenceError: i is not defined”
(5) 存在`暂时性死区`，只要块级作用域内存在let命令，它所声明的变量就“绑定