---
title: 调研 flv.js
date: 2017-07-17 15:17:16
categories: [技术类-前端]
tags: [JavaScript, 插件]
---
## 什么是`flv.js`？
&emsp;&emsp;`flv.js`是一个不借助于`Flash`却可以实现在浏览器上播放`HTML5 Flash流媒体视频(FLV)`的，由纯原生的JavaScript编写的`js插件`。
&emsp;&emsp;基本原理是通过将 FLV 文件流转码复用成 ISO BMFF（MP4 碎片）片段，然后通过 Media Source Extensions（API） 将 MP4 片段喂进浏览器。它是前`bilibili`的员工受到`hls.js`的启发而创作的一个开源项目，[github入口请点击这里](https://github.com/Bilibili/flv.js)！

&emsp;&emsp;本文将顺序介绍`flv.js`的功能、优点、缺点，并在最后给出一个使用`flv.js`的实例。

## `flv.js`的功能
+ 它是一个具有`H.264 + AAC`编码器播放功能的FLV 容器
+ 多部分分段视频播放
+ HTTP FLV 低延迟实时流播放
+ FLV 通过 WebSocket 实时流播放
+ 兼容 Chrome, FireFox, Safari 10, IE11 和 Edge
+ 开销很低，使用了浏览器的硬件加速

## `flv.js`的优点
+ HTML5 原生仅支持播放 mp4/webm 格式，flv.js 实现了在HTML5上播放FLV格式视频
+ 建立在采用了硬件加速的Video标签上，因此性能好，支持高清
+ 在 HTML5 上支持了延迟极低的 HTTP FLV 播放，解决了对`Flash`的依赖问题
+ 支持`录播`和`直播`

## `flv.js`的缺点
+ FLV里所包含的视频编码必须是H.264，音频编码必须是AAC或MP3， IE11和Edge浏览器不支持MP3音频编码，所以FLV里采用的编码最好是H.264+AAC。
+ 在录播上依赖于原生 HTML5 的Video标签和Media Source Extensions API。
+ 在直播上不仅依赖于原生 HTML5 的Video标签和Media Source Extensions  API，同时依赖 HTTP FLV 或者 WebSocket 中的一种协议来传输FLV。其中 HTTP FLV 需通过流式IO去拉取数据，比如fetch或者stream。
+ flv.min.js 文件gzip压缩后与flash播放器gzip压缩后的大小相差无几。
+ 由于依赖Media Source Extensions  API，目前所有iOS和Android4.4.4以下里的浏览器都不支持，也就是说目前对于移动端flv.js基本是不能用的。

## 使用实例