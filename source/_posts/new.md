---
title: JavaScript 中 new 操作符的执行顺序 
date: 2018-11-04 12:24:18
categories: [技术类-前端]
tags: [JavaScript]
---
&emsp;&emsp;以前在学习 JavaScript 构造函数的时候，并没有深入地去研究 new 操作符究竟做了什么事情，也没有深入了解构造返回值的规则，本篇文章详细解释了这两个问题。

## <font style="color: #f00;">new 操作符做了什么？？？</font>
&emsp;&emsp;用 new 调用构造函数时 new 的操作顺序：

+ （一）创建一个新对象 X；
+ （二）将新对象 X 的属性 __proto__ 指向构造函数的 prototype 指向的地方；
+ （三）将构造函数中的 this 绑定为新对象 X；
+ （四）执行构造函数中的代码，比如 this.a = 'xx' 等等；
+ （五）判断构造函数中是否有 return 语句，没有的话就返回新对象 X，有的话先判断 return 后面的值是否为引用类型，是引用类型的话就返回该值，否则依然返回新对象 X。

## <font style="color: #f00;">注意</font>
&emsp;&emsp;（一）对于构造函数，如果没有通过 new 去调用，那么实际该函数返回什么由函数中 return 的具体值决定，即有 return 语句的话 return 什么就返回什么，没有 return 语句的话就返回 undefined；

&emsp;&emsp;（二）但是如果用 new 去调用构造函数，如果没有 return 语句，那就返回由 new 创建的那个对象，有 return 语句的话，先判断 return 后面的是不是引用类型的指，如果是引用类型的值，就返回该值，否则依然返回由 new 创建的那个对象。