---
title: js 实现 call & apply & bind
date: 2019-04-16 01:22:24
categories: [技术类-前端]
tags: [JavaScript]
---
&emsp;&emsp;最近发现某条的前端面试很喜欢让人手写实现 bind。实际上，只要对原生 js 够熟悉的话，别说是实现 bind，连 call 还有 apply 的实现也是很简单的，本篇文章主要是记录下本人实现的代码。
## <font style="color: #f00;">(一) 手写实现 call</font>
```javascript
Function.prototype.myCall = function (context) {
  let args = [...arguments].slice(1)
  context[this.name] = this
  let res = context[this.name](...args)
  delete context[this.name]
  return res
}
```
## <font style="color: #f00;">(二) 手写实现 apply</font>
```javascript
Function.prototype.myApply = function (context, args) {
  context[this.name] = this
  let res = context[this.name](...args)
  delete context[this.name]
  return res
}
```
## <font style="color: #f00;">(三) 手写实现 bind</font>
```javascript
Function.prototype.myBind = function (context) {
  let that = this
  let args = [...arguments].slice(1)
  return function () {
    // 调用了前面手写的 apply
    return that.myApply(context, args.concat([...arguments]))
  }
}
```