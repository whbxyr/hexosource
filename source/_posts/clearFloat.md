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