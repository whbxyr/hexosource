---
title: path.join 与 path.resolve 的区别
date: 2017-05-31 21:07:48
categories: [技术类-前端]
tags: [node,  JavaScript]
---
## <font color="#f00">何为 path ？</font>
&emsp;&emsp;`path`是`node`提供的用于处理文件路径的小工具，我们可以通过以下方式引入该模块：
```javascript
var path = require('path');
```
&emsp;&emsp;`path.join`以及`path.resolve`便是该模块中的两个方法。官方对这两个方法的定义分别如下：

|path.join([path1][, path2][, ...])|path.resolve([from ...], to)|
|:-----:|:-----:|
|用于连接路径。该方法的主要用途在于，会正确使用当前系统的路径分隔符，Unix系统是"/"，Windows系统是"\"。|将 to 参数解析为绝对路径。|

&emsp;&emsp;很多同学在使用中分不清楚它们两者之间的区别。接下来，我们就来看看它们二者之间的区别。
## <font color="#f00">path.join 与 path.resolve 的区别</font>
+ <font color="#00f">相同点</font>

**1.**二者都是输出路径字符串

**2**.二者都能正确处理父级标识`'../'`
例子：现假设在 ububtu 的 home 目录的 test 文件夹里新建一个 path.js 文件。（以下均以这个文件作为例子讲解）
```javascript
/**
 * path.js
 */
var path = require('path');

// 使用 path.join
console.log(path.join(__dirname, '../hexo'));
// 输出 /home/whbxyr/hexo

// 使用 path.resolve
console.log(path.resolve(__dirname, '../hexo'));
// 输出 /home/whbxyr/hexo
```
&emsp;&emsp;可见二者均是将`'../'`视为父级目录，而不是当成简单的字符串。

+ <font color="#00f">不同点</font>

**1.**path.join 只是简单的连接路径，而 path.resolve 则是将最后一个参数的路径解析为`绝对路径`。
```javascript
/**
 * path.js
 */
var path = require('path');

// 使用 path.join
console.log(path.join('test', './hexo'));
// 输出 test/hexo

// 使用 path.resolve
console.log(path.resolve('test', './hexo'));
// 输出 /home/whbxyr/test/test/hexo
```

**2.**path.join 将`'/'`视为简单的当前路径，而 path.resolve 将`'/'`视为根目录。
```javascript
/**
 * path.js
 */
var path = require('path');

// 使用 path.join
console.log(path.join(__dirname, '/hexo'));
// 输出 /home/whbxyr/test/hexo

// 使用 path.resolve
console.log(path.resolve(__dirname, '/hexo'));
// 输出 /hexo
```
&emsp;&emsp;path.join 的第二个参数前面有个`'/'`，但是输出的结果也仅仅是简单的当前路径的连接，而 path.resolve 的第二个参数前面也有`'/'`，但是由于它将其视为根目录，因此第二个参数的绝对路径便直接是根目录下的。

## <font color="#f00">总结</font>
&emsp;&emsp;path.join 与 path.resolve 虽然有时候运行结果是相同的，有些情况下使用哪个都可以，但是它们二者之间还是有本质区别的，我们在平常的使用中应该要多加注意，加以区分。