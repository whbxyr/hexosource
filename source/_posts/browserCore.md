---
title: 各大浏览器内核(渲染引擎/JS引擎)
date: 2017-04-15 15:17:16
categories: [技术类-前端]
tags: [浏览器]
---
## 简介
&emsp;&emsp;在介绍各个主流的浏览器内核之前，首先介绍浏览器内核的**组成**及其**工作原理**/**作用**。

&emsp;&emsp;**渲染引擎**(又叫**排版引擎**)以及**JS引擎**是浏览器内核的两个组成部分。

&emsp;&emsp;**渲染引擎**负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入 CSS 等），以及计算网页的显示方式，然后会输出至显示器或打印机。它也可以借助插件（一种浏览器扩展）显示其他类型数据，例如使用PDF阅读器插件（所以想做浏览器插件的同学必须知道浏览器的工作原理），可以显示PDF格式。

&emsp;&emsp;**JS引擎**负责对JavaScript进行解释、编译和执行，以使网页达到一些动态的效果。

## 渲染引擎
一. **Trident**/**EdgeHTML** -> (IE浏览器/Edge浏览器)

&emsp;&emsp;**Trident**是微软的IE浏览器使用的渲染引擎，它是从从早期一款商业性的专利网页浏览器Spyglass Mosaic派生出来的。Window 10发布之后，微软将其内置的浏览器命名为Edge，而Edge浏览器的渲染引擎便是**EdgeHTML**。

二. **WebKit**（WebCore） -> (Safari浏览器)

&emsp;&emsp;**WebKit**是苹果公司的Safari浏览器使用的渲染引擎，是KDE（Linux桌面系统）小组的KHTML引擎的一个开源的分支。

三. **Chromium**/**Blink** -> (Chrome浏览器)

&emsp;&emsp;**Chromium**是谷歌公司的一个开源浏览器项目，每隔一段时间，谷歌会在最新的比较稳定的Chromium版本上加入一些其他的功能使之成为新版本的Chrome浏览器。而Chromium的渲染引擎采用的是**Blink**，其前身是**Webkit**。**Blink**是从早期的**Webkit**项目（而非后来的**Webkit2**）复制出来另外维护的一个新的项目。

四. **Presto** -> (Opera浏览器)

&emsp;&emsp;**Presto**是挪威Opera Software ASA公司的Opera浏览器使用的渲染引擎，后来该公司为了减少研发成本，跟随Chrome浏览器先后将渲染引擎改为**Chromium** 和 **Blink**。

五. **Gecko** -> (FireFox浏览器)

&emsp;&emsp;**Gecko**是Mozilla公司的FireFox浏览器使用的渲染引擎，它是一款开源的跨平台渲染引擎，可以在Windows、 BSD、Linux 和 Mac OS X 中使用。

## JS引擎
#### 一. 微软的JS引擎
|JScript|Chakra|
|:---:|:---:|
|IE3.0-IE8.0|IE9+|

#### 二. 苹果的JS引擎
|Nitro|
|:---:|
|为Safari 4编写|

#### 三. Google的JS引擎
|V8|
|:---:|
|开放源代码，由Google丹麦开发，是Chrome浏览器的一部分|

#### 四. Opera的JS引擎
|Linear A|Linear B|Futhark|Carakan|
|:---:|:---:|:---:|:---:|
|Opera 4.0～6.1|Opera 7.0～9.2|Opera 9.5～10.2|Opera10.50+|

#### 五. Mozilla的JS引擎
|SpiderMonkey|Rhino|TraceMonkey|JaegerMonkey|JavaScriptCore、WebKit|IonMonkey|OdinMonkey|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|第一款JavaScript引擎|完全以Java编写|基于实时编译的引擎|结合追踪和组合码技术大幅提高性能|用于Mozilla Firefox 4.0以上版本|可以对JavaScript编译后的结果进行优化|可以对asm.js进行优化，用于Mozilla Firefox 22.0以上版本|