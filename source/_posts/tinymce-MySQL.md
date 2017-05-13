---
title: 利用tinymce编写网页文章并插入到MySQL数据库
date: 2016-10-04 18:23:22
categories: [技术类-前端]
tags: [插件]
---
#### tinymce是一个轻量级的基于浏览器的所见即所得编辑器，由JavaScript写成。以下我将为大家介绍如何使用JavaScript的Ajax、配合使用php将tinymce中编写的文章经过转码后顺利存储进MySQL数据库。（将文章数据库表设计为只有两个字段，一个是int型的自增长id字段，另一个是text类型的article字段）
##### 　　第一步：在你的网站根目录（或者其他网站目录也行）下新建一个editor.html文件，编辑代码如下：
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>tinymce编辑文章</title>
    <script src="http://cdn.tinymce.com/4/tinymce.min.js"><script>
    <script src="./event.js"></script>
    <script src="./htmlcode.js"></script>
</head>
<body>
    <textarea id="editor" name="content"></textarea>
    <input id="handin" type="button" value="提交">
</body>
</html>
```
 　　这个文件将tinymce文章编辑器插件成功应用到我们的页面当中,并添加了一个用于提交文章到数据库的“提交”按钮。

 　　其中引入的event.js文件代码如下，应将其置于同一目录下。它是一个为DOM添加事件处理的兼容脚本。
```javascript
/**
 * 为DOM添加事件处理的兼容脚本
 * /
var EventUtil = {
    // 添加句柄
    addHandler: function (element, type, handler) {
        if (element.addEventListener) {
            element.addEventListener(type, handler, false);
        }
        else if (element.attachEvent) {
            element.attachEvent('on'+type, handler);
        }
        else {
            element['on'+type] = handler;
        }
    }
} 
```
 　　其中引入的htmlcode.js文件代码如下，同样将其置于editor.html同一目录下。它是一个实现html转码的脚本。
```javascript
/**
 * 用浏览器内部转换器实现
 * javascript处理HTML的Encode(转码)和Decode(解码)
 */
var HtmlUtil = {
    // 用浏览器内部转换器实现html转码
    // 首先动态创建一个容器标签元素，如DIV，
    // 然后将要转换的字符串设置为这个元素的innerText(ie支持)
    // 或者textContent(火狐，google支持)，最后返回这个元素的innerHTML，
    // 即得到经过HTML编码转换的字符串了。
        htmlEncode: function (html) {  
            var div = document.createElement('div');  
            div.appendChild(document.createTextNode(html));  
            return div.innerHTML;
        },
        // 用浏览器内部转换器实现html解码
        // 首先动态创建一个容器标签元素，如DIV，
        // 然后将要转换的字符串设置为这个元素的innerHTML(ie，火狐，google都支持)，
        // 最后返回这个元素的innerText(ie支持)或者textContent(火狐，google支持)，
        // 即得到经过HTML解码的字符串了。
        htmlDecode: function (text) {  
            var div = document.createElement('div');  
            div.innerHTML = text;  
            return div.innerText || div.textContent;
        }
    }
```
##### 　　第二步：将以下代码插入到editor.html文件的底部，其实就是一些用script标签包起来的JavaScript代码。
```javascript
EventUtil.addHandler(document.getElementById('handin'), 'click', function () {
    // 创建一个XMLHttpRequest对象
    var request = new XMLHttpRequest();
    // 对articleToDB.php文件发出POST请求
    request.open('POST', './articleToDB.php');


    // ajax传过去的参数都会被处理为字符串，因此不需要再将参数转换为字符串
    // 先进行“去\n”操作，再进行html编码，使用的是浏览器内部转换器
    var article = HtmlUtil.htmlEncode((tinyMCE.activeEditor.getContent()).replace(/[\n]/ig, ''));
    // tinyMCE.activeEditor.getContent()方法会在我们写的文章里面的每一个\前再加一个\
    // 当我们在文章里只写了一个\时，
    // tinyMCE.activeEditor.getContent()方法给我们多加了一个\
    // 此时就有了两个\，但是正则表达式不会匹配第一个作为转义字符作用的反斜杠'\'，
    // 在用正则表达式匹配字符串时，转义符相当于不存在
    // 因此在这里用正则匹配时就只需要4个转义符
    // 用replace()方法将其替换为4个\
    // 即符合mysql数据库3个转义符转义一个字符的规则
    article = article.replace(/\\/g, '\\\\');
    // 将获取的html代码中的"替换为\\\"
    // 这样ajax才能正确读取参数并将其完整插入数据库
    article = article.replace(/["]/g, '\\\"');
    // 对处理完的字符串进行最终的编码，去除取值符等敏感字符对ajax传值的影响
    article = encodeURIComponent(article);
    // 拼接请求主体
    var data = 'article=' + article;
    // POST请求方式必须设置的请求头格式
    request.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
    request.send(data);


    request.onreadystatechange = function () {
        if (request.readyState === 4) {
            if (request.status === 200) {
                // 弹出相关文字提示
                alert(request.responseText);
            }
            else {
                // 当请求不到数据时就会导致错误，用以下方法显示
                alert('发生错误：' + request.status);
            }
        }
    }
});
```
##### 　　第三步：编写articleToDB.php文件实现文章插入MySQL数据库，并将其置于editor.html同一目录下。
```php
    <?php
    header('Content-Type: application/json; charset=utf-8');
    // 若取不到值，那么进行相应的信息反馈并结束
    if (!isset($_POST['article']) || empty($_POST['article'])) {
        echo '您还没有编辑文章';
        exit;
    }
    // 面向对象的建立数据库连接的方法
    // 应该填写服务器数据库的主机名，用户名以及相对应的密码
    // 比如$db = new mysqli('localhost','root','123456789');
    $db = new mysqli('主机名','用户名','密码');

    if (mysqli_connect_errno()) {
        echo '连接数据库失败，请稍后重试！';
        exit;
    }
    // 判断数据库是否存在
    $select = $db->select_db('article');
    if (!$select) {
        // 创建数据库失败后的处理提示
        $createDB_query = 'create database article';
        $createDB = $db->query($createDB_query);
        if (!$createDB) {
            echo '创建数据库失败';
            exit;
        }
        //使用数据库失败后的处理提示
        $useDB_query = 'use article';
        $useDB = $db->query($useDB_query);
        if (!$useDB) {
            echo '使用数据库失败';
            exit;
        }
        else {
	        echo '成功';
            exit;
        }
    }
    // 设置数据库编码
    $db->query('set names utf8');
    // 此处在接收到经过编码的article后，通过=操作符，实现了解码
    $article = $_POST['article'];
    $insert_query = 'insert into text2 values(NULL, "'.$article.'")';
    $insert_result = $db->query($insert_query);
    // 对返回结果进行判断
    if ($insert_result) {
        echo '成功！管理员文章发布成功！';
    }
    else {
        echo '失败！管理员文章发布失败！';
    }
    // 关闭数据库连接
    $db->close();
```
#### 　　以上三步便是利用JavaScript的Ajax、配合使用php将tinymce中编写的文章经过转码后顺利存储进MySQL数据库的全过程，需要注意的点有以下几个：
 　　1. html代码必须经过转码后才能插入数据库; 

 　　2. 字符串'\\\'长度为1而不是2;

 　　3. 正则表达式匹配字符串时并不匹配起转义字符作用的反斜杠'\';

 　　4. 在插入MySQL数据库时，3个转义字符转义一个字符