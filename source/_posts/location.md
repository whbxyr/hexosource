---
title: window.location 对象详解
date: 2017-08-27 12:17:16
categories: [技术类-前端]
tags: [JavaScript]
---
### <font style="color: #f00;">window.location 的属性</font>
&emsp;&emsp;本文通过一个示例 url，讲述`window.location`的各个属性。假设该url如下：
```
https://www.baidu.com/s?wd=location.hash&rsv_spt=1&rsv_iqid=0xa78ca3100003b820&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&rqlang=cn&tn=baiduhome_pg&rsv_enter=1&rsv_t=6decUG6FR%2BbaSb%2FPMsTCZmiKd2aU05OANFzmoTyRD5hNNitxtu1jTmnpxMiD1NK6nql%2F&oq=location.hash&inputT=6&rsv_sug3=9&rsv_sug1=8&rsv_sug7=100&rsv_sug2=0&rsv_pq=a80d243900049765&rsv_sug4=190534#imhere
```
&emsp;&emsp;当我们访问这个网页的时候，在控制台运行以下代码：
```javascript
// 控制台中输入以下代码：
JSON.stringify(window.location);
/* 可以获得以下字符串（为方便查看，手动缩进规范处理了下）
"{
  "href":"https://www.baidu.com/s?wd=location.hash&rsv_spt=1&rsv_iqid=0xa78ca3100003b820&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&rqlang=cn&tn=baiduhome_pg&rsv_enter=1&rsv_t=6decUG6FR%2BbaSb%2FPMsTCZmiKd2aU05OANFzmoTyRD5hNNitxtu1jTmnpxMiD1NK6nql%2F&oq=location.hash&inputT=6&rsv_sug3=9&rsv_sug1=8&rsv_sug7=100&rsv_sug2=0&rsv_pq=a80d243900049765&rsv_sug4=190534#imhere",
  "ancestorOrigins":{},
  "origin":"https://www.baidu.com",
  "protocol":"https:",
  "host":"www.baidu.com",
  "hostname":"www.baidu.com",
  "port":"",
  "pathname":"/s",
  "search":"?wd=location.hash&rsv_spt=1&rsv_iqid=0xa78ca3100003b820&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&rqlang=cn&tn=baiduhome_pg&rsv_enter=1&rsv_t=6decUG6FR%2BbaSb%2FPMsTCZmiKd2aU05OANFzmoTyRD5hNNitxtu1jTmnpxMiD1NK6nql%2F&oq=location.hash&inputT=6&rsv_sug3=9&rsv_sug1=8&rsv_sug7=100&rsv_sug2=0&rsv_pq=a80d243900049765&rsv_sug4=190534",
  "hash":"#imhere"
}"
*/
```
&emsp;&emsp;由上可得 window.location 的10个属性：
&emsp;&emsp;href 			/ ancestorOrigins 	/ origin 		/	protocol 	/	host
&emsp;&emsp;hostname 	/ port 						/ pathname 	/ 	search 		/	hash
1. **window.location.href**
整个URl字符串(在浏览器中就是完整的地址栏)
本例返回值: https://www.baidu.com/s?wd=location.hash&rsv_spt=1&rsv_iqid=0xa78ca3100003b820&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&rqlang=cn&tn=baiduhome_pg&rsv_enter=1&rsv_t=6decUG6FR%2BbaSb%2FPMsTCZmiKd2aU05OANFzmoTyRD5hNNitxtu1jTmnpxMiD1NK6nql%2F&oq=location.hash&inputT=6&rsv_sug3=9&rsv_sug1=8&rsv_sug7=100&rsv_sug2=0&rsv_pq=a80d243900049765&rsv_sug4=190534
2. **window.location.protocol**
URL 的协议部分
本例返回值: https:
3. **window.location.host**
URL 的主机部分
本例返回值: www.baidu.com
4. **window.location.hostname**
URL 的主机部分，在这里 window.location.hostname === window.location.host 
本例返回值: www.baidu.com
5. **window.location.port**
URL 的端口部分
如果采用默认的80端口(即使添加了:80)，那么返回值并不是默认的80而是空字符
本例返回值: ""
6. **window.location.pathname**
URL 的路径部分(就是文件地址)
本例返回值: /s
7. **window.location.search**
查询(参数)部分
除了给动态语言赋值以外，我们同样可以给静态页面,并使用javascript来获得相信应的参数值
本例返回值: ?wd=location.hash&rsv_spt=1&rsv_iqid=0xa78ca3100003b820&issp=1&f=8&rsv_bp=1&rsv_idx=2&ie=utf-8&rqlang=cn&tn=baiduhome_pg&rsv_enter=1&rsv_t=6decUG6FR%2BbaSb%2FPMsTCZmiKd2aU05OANFzmoTyRD5hNNitxtu1jTmnpxMiD1NK6nql%2F&oq=location.hash&inputT=6&rsv_sug3=9&rsv_sug1=8&rsv_sug7=100&rsv_sug2=0&rsv_pq=a80d243900049765&rsv_sug4=190534
8. **window.location.hash**
锚点
本例返回值: #imhere

### <font style="color: #f00;">window.location 改变当前页面 url 的方法</font>
1. window.location = 'xxx';
2. location = 'xxx';
3. window.location.href = 'xxx';
4. location.href = 'xxx';
5. window.replace('xxx');

&emsp;&emsp;这五种方法都会让浏览器的历史记录里面多一项，但是有一个较大的区别就是前四种方法依然可以通过使用浏览器的**“前进”**和**“后退”**按钮回到替换url之前的原始url，但是对于最后一种方法 window.replace(url)，则将失去浏览器的**“前进”**和**“后退”**按钮功能，因为该方法通过指定URL替换当前缓存在历史里（客户端）的项目，因此当使用replace方法之后，你不能通过**“前进”**和**“后退”**来访问已经被替换的URL。

### <font style="color: #f00;">window.location.reload([bForceGet])</font>
&emsp;&emsp;window.location.reload([bForceGet])方法接收一个参数，默认为false，即刷新当前页面并如果有缓存的话就从缓存里面读取页面。当传入这个方法的参数为true时，则表示强刷页面，效果与用户直接 Ctrl + F5 的效果是一样的，即直接从服务器获取最新的页面。

### <font style="color: #f00;">使用 window.location.hash 欺骗浏览器</font>
&emsp;&emsp;有这么一个场景，就是在一个页面里面，浏览到了你想收藏的地方，这时候你收藏了链接，下次再打开的时候，却发现还是要从头开始找那个你想收藏的地方。但是，使用 window.location.hash 可以解决这个问题。
&emsp;&emsp;解决方法就是给链接加上一个`hash`值，每次打开链接的时候使用js去获取这个`hash`值，并根据不同的`hash`值显示不同的页面。这样，还可以欺骗浏览器，让浏览器以为加了`hash`的链接是一个新的页面，这样就可以充分使用浏览器的**“前进”**和**“后退”**功能了。
```
// 示意代码
window.onload = function () {
  var hash = window.location.hash;
  switch (hash) {
    case 'part1':
      showPart1();
      break;
    case 'part2':
      showPart2();
      break;
    case '':
      showPart0();
      break;
    default:
      showPart0();
      break; 
  }
};
```

### <font style="color: #f00;">注意</font>
window.location === document.location 在没有使用 iframe 时，结果为 true，即两者没有什么区别，但是一旦页面中使用了 iframe 框架，就会有不同了，在 iframe 中的 window.location 等同于 top.location，但是不一定与 document.location 等同。

### <font style="color: #f00;">总结</font>
&emsp;&emsp;合理利用`window.location`可以实现很多有趣的功能，比如在 a 标签中使用 href="#" 可以实现快速回到页面顶部。