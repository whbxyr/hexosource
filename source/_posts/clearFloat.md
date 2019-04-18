---
title: 清除子元素浮动造成的影响
date: 2016-04-22 10:21:34
categories: [技术类-前端]
tags: [html, css]
---
#### <font color="#f00">第一种：</font>父级div设置高度
```html
<style>
.father {
    height: 200px;
}
.son {
    float: left;
    width: 20%;
    height: 200px;
}
</style>
<div class="father">
    <div class="son">浮动元素</div>
</div>
```
&emsp;&emsp;这种方法比较蠢，只是假象，如果子元素浮动的高度大于父元素，那么子元素依然会超出父元素的底部造成父元素高度塌陷的问题。
#### <font color="#f00">第二种：</font>父级内部结尾处添加空的`clear: both`的div标签
```html
<style>
.son {
    float: left;
    width: 20%;
    height: 200px;
}
.clearfloat {
    clear: both;
}
</style>
<div class="father">
    <div class="son">浮动元素</div>
    <div class="clearfloat"></div>
</div>
```
&emsp;&emsp;这种方法实际上就是在底部强行加入了一个清除浮动的子元素。
#### <font color="#f00">第三种：</font>父级使用伪类，类似第二种方法
```html
<style>
.father {
    *zoom: 1;
}
.father:after {
    content: '';
    display: block;
    clear: both;
    height: 0;
    overflow: hidden;
}
.son {
    float: left;
    width: 20%;
    height: 200px;
}
</style>
<div class="father">
    <div class="son"></div>
</div>
```
或者使用下面的方法，也是使用了伪类
```html
<style>
.father {
    *zoom: 1;
}
.father:after {
    content: '';
    display: table;
    clear: both;
}
.son {
    float: left;
    width: 20%;
    height: 200px;
}
</style>
<div class="father">
    <div class="son"></div>
</div>
```
&emsp;&emsp;换汤不换药，底部的伪元素就相当于一个匿名的 div，只是他被附上了 clear: both; 的属性。
#### <font color="#f00">第四种：</font>给父级添加`overflow: hidden/auto/scroll;`、`float: left/right;`、`position: absolute;`、`display: inline-block;`、`zoom: 1;`五个CSS属性其中之一
```html
<style>
.father {
    overflow: hidden/auto/scroll;
    /*float: left/right;*/
    /*position: absolute;*/
    /*display: inline-block;*/
    /*zoom: 1;*/
}
.son {
    float: left;
    width: 20%;
    height: 200px;
}
</style>
<div class="father">
    <div class="son"></div>
</div>
```
&emsp;&emsp;最后这个则是运用了 BFC（Block Formatting Context）的原理，使得父元素的表现就跟```<html>```一样，能包含万物（包括浮动了的子元素）。