---
title: 理解语义化
date: 2017-07-22 19:17:16
categories: [技术类-前端]
tags: [html]
---
&emsp;&emsp;以前开始学前端、作为一名萌新的时候，一上来就是狂刷 html 的各种标签，拿到 `<div>` `<span>` 就是学习如何实现各种炫酷效果，很多实现都基本会首选这两个标签，但是却基本不考虑`语义化`这种看似“很玄”的东西，但是随着自己接触的项目逐渐增多以及现 HTML 5 的流行，慢慢发现如果有更高追求的话，`语义化`就是一个不得不考虑的点。

## 语义化的定义
&emsp;&emsp;上维基百科搜了一下，它对`语义化`的定义如下：
> 语义化是前端开发里面的一个专用术语，其优点在于标签语义化有助于构架良好的html结构，有利于搜索引擎的建立索引、抓取；另外，亦有利于页面在不同的设备上显示尽可能相同；此外，亦有利于构建清晰的机构，有利于团队的开发、维护。

&emsp;&emsp;这段维基百科是2012年编写的，可见语义化并不是一个新概念，在搜索引擎大行其道的`web2.0`时代，甚至是在更早的时候，大家就都已经意识到了`语义化`这个概念了，语义化并不是 HTML 5 专有的概念。
&emsp;&emsp;在我看来，语义化的 html 就是让相应的标签做相应的事情，在没有 CSS 样式的情况下，网页也以正确的文档格式显示。这样不仅便于开发者阅读理解代码以及写出更优雅的代码，还能页面内容结构化，让网络爬虫很好地解析网页，实现 SEO 优化。

## 总结语义化的作用
+ 在没有 CSS 样式的时候能够清楚体现页面的结构，增强可读性
+ 语义化的 html 让开发者更容易看明白，便于团队的开发和维护，提高团队的开发效率
+ 有利于网页在多个终端的浏览器内正常渲染、显示
+ 有利于网页的 SEO 优化，结构语义化的 html 能够被搜素引擎更好地理解和爬取

## 怎么写语义化的 html
&emsp;&emsp;实现语义化的 html，关键就是要实现`结构与样式分离`。站在 html 的角度上，它只告诉我们它包含的内容是什么，意义是什么，但是它的样式不应该由自己来决定，而应该由 CSS 来决定。
&emsp;&emsp;总的来说，写语义化 的 html 其实并不苦难。主要包含以下几点：
1. 掌握 html 中各个标签的语义，比如`<h1>`~`<h6>`表示标题，`<p>`表示段落，`<header>`、`<main>`、`<footer>`分别表示页面的头部、主体以及页脚等等。
2. 在构建 html 的时候，提前想好某块内容的结构语义，判断它是一个标题还是段落或者一篇文章等等。
3. 在实际敲 html 代码的时候，根据具体语义选择对应的标签就可以了。

&emsp;&emsp;下面图片展示的是一个语义化 html 的例子。
![语义化的 html](semantic/1.png)