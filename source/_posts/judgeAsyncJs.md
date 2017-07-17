---
title: 动态加载js脚本并判断是否加载完成
date: 2017-07-17 19:17:16
categories: [技术类-前端]
tags: [JavaScript]
---
&emsp;&emsp;异步加载js的方法可以是先创建一个`<script>`标签，再给这个标签添加`src`属性并将该元素添加到文档后，这时候浏览器就会开始异步加载JavaScript文件。它与图像不同的是，为图像创建的`<img>`标签，一旦给予了它`src`属性，它就会立马加载图片，而不管`<img>`标签是否已经被插入文档中。

&emsp;&emsp;因此，对于`<script>`元素而言，指定`src`属性和指定事件处理程序的先后顺序就不重要了，在这一点上，与`<link>`标签的表现是一致的，`<link>`标签在`href`属性被赋值后也不会立即下载样式表，只有将该元素添加到文档中后才会开始下载，因此指定`href`属性和指定事件处理程序的先后顺序也不重要了；而对于`<img>`元素而言，则必须先指定事件处理程序，再赋予`src`属性，否则会出现捕获不到加载完成事件的情况。

## 动态加载js文件并判断状态

#### （一）监听 load 事件
&emsp;&emsp;其中使用了跨浏览器的 EventUtil 对象，详见我的文章 [兼容浏览器的事件处理程序 EventUtil](http://www.whbxyr.cn/2016/06/24/EventUtil/)！
```javascript
// 待文档及资源加载完毕后，再异步动态加载一个js文件
EventUtil.addHandler(window, 'load', function () {
  // 创建一个<script>标签
  var script = document.createElement('script');
  // 添加 load 事件的监听
  EventUtil.addHandler(script, 'load', function () {
    console.log('异步脚本加载完毕');
  });
  // 赋予 src 属性
  script.src = 'example.js';
  // 将<script>标签插入到文档中后开始加载js文件
  document.body.appendChild(script);
});
```
&emsp;&emsp;以上便是异步加载js文件并判断是否加载完成的方法之一。

#### （二）监听对象的 readyState 属性
&emsp;&emsp;其中依然使用了跨浏览器的 EventUtil 对象。
```javascript
// 待文档及资源加载完毕后，再异步动态加载一个js文件
EventUtil.addHandler(window, 'load', function () {
  // 创建一个<script>标签
  var script = document.createElement('script');
  // 添加 readystatechange 事件的监听
  EventUtil.addHandler(script, 'readystatechange', function (event) {
    // 获取事件
    event = EventUtil.getEvent(event);
    // 获取发生相应事件的 DOM 对象
    var target = EventUtil.getTarget(event);
    
    // 根据对象的 readyState 属性的值来判断加载是否完成
    if (target.readyState === 'loaded' || target.readyState === 'complete') {
      EventUtil.removeHandler(target, 'readystatechange', arguments.callee);
      console.log('异步脚本加载完毕');
    }
  });
  
  // 赋予 src 属性
  script.src = 'example.js';
  // 将<script>标签插入到文档中后开始加载js文件
  document.body.appendChild(script);
});
```
&emsp;&emsp;这种方法主要是依据 readystatechange 事件来判断动态加载的js文件是否加载完成。