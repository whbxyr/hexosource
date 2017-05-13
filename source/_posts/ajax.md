---
title: ajax 的兼容使用方法
date: 2015-04-18 13:45:07
categories: [技术类-前端]
tags: [ajax, JavaScript]
---
## <font color="#f00">简介</font>
&emsp;&emsp;Ajax技术的核心是XMLHttpRequest对象（简称XHR对象），这是由微软首先引入的一个特性。Ajax是对`Asynchronous JavaScript + XML`的简写，但Ajax通信与数据格式无关，这种技术是无须刷新页面即可从服务器取得数据，但不一定是XML数据。

&emsp;&emsp;IE5是第一款引入`XHR`对象的浏览器。在IE5中，`XHR`对象是通过MSXML库中的一个ActiveX对象实现的。因此，在IE中可能会遇到三种不同版本的XHR对象，即`MSXML2.XMLHttp`、`MSXML2.XMLHttp.3.0`、`MSXML2.XMLHttp.6.0`。因此，为了兼容浏览器**IE 6-**，必须写一个函数去根据IE中可用的MSXML库的情况创建最新版本的XHR对象。

&emsp;&emsp;而 IE 7+、Firefox、Opera、Chrome 和 Safari 都支持原生的XHR对象，在这些浏览器中创建XHR对象要像下面这样使用`XMLHttpRequest`构造函数。
```javascript
var xhr = new XMLHttpRequest();
```
## <font color="#f00">兼容函数`createXHR`</font>
```javascript
function createXHR() {
    if (typeof XMLHttpRequest !== 'undefined') {
        return new XMLHttpRequst();
    }
    else if (typeof ActiveXObject !== 'undefined') {
        if (typeof arguments.callee.activeXString !== 'string'){
            var versions = ['MSXML2.XMLHttp', 'MSXML2.XMLHttp.3.0', 'MSXML2.XMLHttp.6.0'];
            var i, len;
            for (var i = 0, len = versions.length; i < len; i++) {
                try {
                    new ActiveXObject(versions[i]);
                    arguments.callee.activeXString = versions[i];
                    break;
                }
                catch (ex) {
                    // 跳过
                }
            }
        }
        return new ActiveXObject(arguments.callee.activeXObject);
    }
    else {
        throw new Error('No XHR object available');
    }
}
```
&emsp;&emsp;这个函数首先检测原生XHR对象是否存在，如果它存在则返回它的新实例，否则检测ActiveXObject对象。如果这两种对象都不存在，就抛出一个错误。然后，就可以使用下面的代码在所有的浏览器中创建`XHR`对象了。
```javascript
var xhr = createXHR();
```
## <font color="#f00">XHR的具体使用方法</font>
&emsp;&emsp;XHR对象的通用使用方法如下：
```javascript
var xhr = createXHR();
// 异步的ajax请求可以检测XHR对象的readyState属性，该属性表示请求/响应过程的当前活动阶段
// 0：未初始化。刚创建XHR对象，尚未调用open()方法
// 1：启动。已经调用open()方法，但尚未调用send()方法
// 2：发送。已经调用send()方法，但尚未接收到响应
// 3：接收。已经接收到部分响应数据
// 4：完成。已经接收到全部响应数据，而且已经可以在客户端使用了
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            // 服务器响应的数据会自动填充XHR对象的属性，相关的属性如下：
            // responseText：作为响应主体被返回的文本
            // responseXMl：如果响应的内容类型是“text/xml”或“application/xml”，这个属性中将保存包含着响应数据的 XML DOM 文档
            // status：响应的 HTTP 状态
            // statusText：HTTP 状态的说明
            alert(xhr.responseText);
            // 调用getResponseHeader()或者getAllResponseHeaders()可以取得相应的响应头部信息
            var myHeader = xhr.getResponseHeader('MyHeader');
            var allHeaders = xhr.getAllResponseHeaders();
        }
        else {
            alert('Request was unsuccessful: ' + xhr.status);
        }
    }
};
// 查询字符串中每个参数的键和值都必须使用encodeURIComponent()进行编码才追加到URL末尾
function addURLParam(url, name, value) {
    url += (url.indexOf('?') === -1 ? '?' : '&');
    url += encodeURIComponent(name) + '=' + encodeURIComponent(value);
    return url;
}
var url = 'example.php';
// 添加参数
url = addURLParam(url, 'name', 'Ray');
url = addURLParam(url, 'book', 'Green Story');
// 第一个参数可为“get”、“post”等
// 第三个参数为true表示ajax请求为异步的，否则为同步的
xhr.open('get', url, true);
// 若要调用setRequestHeader()方法，必须在open()之后，send()之前
xhr.setRequestHeader('MyHeader', 'MyValue');
xhr.send(null);
// 在接收到响应之前还可以调用abort()方法取消异步请求
// xhr.abort();

// 以上使用的是get的提交方式，以下以一个post提交方式的ajax请求来模拟表单提交
xhr.open('post', 'postexample.php', true);
// 使用post提交方式必须声明这个头部，表示表单提交时的内容类型
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
var form = document.getElementById('user-info');
// 序列化页面中的“表单数据”并发送
xhr.send(serialize(form));
```
## 总结
+ 负责Ajax运作的核心对象是XMLHttpRequest（XHR）对象。
+ XHR对象由微软最早在IE5中引入，用于通过JavaScript从服务器取得XML数据。
+ 在此之后，Firefox、Safari、Chrome 和 Opera都是实现了相同的特性，使XHR成为了Web的一个事实标准。
+ 虽然实现之间存在差异，但XHR对象的基本使用方法在不同浏览器间还是相对规范的，因此可以放心地用在Web开发当中。