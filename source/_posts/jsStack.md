---
title: 使用 js 实现栈并解决正整数"进制转换"的问题
date: 2017-07-10 00:18:27
categories: [技术类-前端]
tags: [JavaScript, 算法]
---
&emsp;&emsp;在这篇文章中，我将结合 js 的构造函数以及 js 的数组，实现数据结构`栈`并利用 js 实现的栈来实现正整数的 `进制转换`算法，即将正整数转换为其他进制的数（包括二进制到十六进制）。

## <font style="color: #f00;">(一) 利用 js 实现数据结构`栈`</font>
&emsp;&emsp;充分利用 ECMAScript 原生的`构造函数`以及原生的数据结构`数组`，可以很容易地实现栈。代码以及注释如下：
```javascript
function Stack() {
	// 初始化栈
	var items = [];
	// 入栈
	this.push = function (item) {
		items.push(item);
	};
	// 出栈
	this.pop = function () {
		return items.pop();
	};
	// 获取栈顶元素
	this.peek = function () {
		return items[items.length - 1];
	};
	// 判空
	this.isEmpty = function () {
		return items.length === 0;
	};
	// 获取栈的大小
	this.size = function () {
		return items.length;
	};
	// 清空栈
	this.clear = function () {
		items = [];
	};
	// 打印栈
	this.print = function () {
		console.log(items.toString());
	};
}
```

## <font style="color: #f00;">(二) 利用栈将正整数转换为其他进制数（2~16）</font>
&emsp;&emsp;只要知道十进制数转换为其他进制数的原理，就可以很简单地利用栈来实现。原理就是通过不断地进行除法，将余数放入栈中，等到原来的十进制数变为零时，再将栈中的数据逐个出站拼接，便能获得我们想要的目标进制数。代码以及注释如下所示：
```javascript
/*
 * 十进制正整数： decNumber
 * 目标进制(base >= 2 && base <= 16)： base
 */
function baseConvert(decNumber, base) {
	// 初始化余数栈
	var remStack = new Stack();
	// 初始化余数
	var rem;
	// 初始化转换结果
	var baseString = '';
	// 解决11到16进制之间的进制数表示问题
	var digits = '0123456789ABCDEF';

	// 计算余数栈
	while (decNumber > 0) {
		rem = decNumber % base;
		remStack.push(rem);
		decNumber = Math.floor(decNumber / base);
	}

	// 计算结果
	while (!remStack.isEmpty()) {
		baseString += digits[remStack.pop()];
	}

	return baseString;
}
```
&emsp;&emsp;另外，只要知道如何将十进制正整数转换为其他进制的数，那么也可以轻松实现任何进制之间的转换。在我看来，分为两个步骤：
&emsp;&emsp;**1.** 将原始进制数a转换为十进制数b
&emsp;&emsp;**2.** 将得到的b再利用上述算法，即可得到最终的目标进制数c