---
title: 详解 JSON 以及 JSON 在JS中的使用
date: 2015-03-16 16:03:50
categories: [技术类-前端]
tags: [JavaScript]
---
## 简介
&emsp;&emsp;`JSON`是一种数据结构而不是一种编程语言，它利用了JS中的一些模式来表示结构化数据，虽然与JS具有相同的语法形式，但是JSON并不从属于JS。另外，并不是只有JS才使用JSON，很对编程语言都有针对JSON的解析器和序列化器。
## JSON的语法
#### 一. 取值类型
&emsp;&emsp;**简单值**： 可以在JSON中表示字符串、数值、布尔值和null，但不支持特殊值`undefined`。
&emsp;&emsp;**对象**： 复杂数据类型，表示一组无序的键值对，每个键值对中的值可以是简单值，也可以是复杂数据类型的值。
&emsp;&emsp;**数组**： 复杂数据类型，表示一组有序的值的列表，可以通过数值索引来访问其中的值，该值也可以是简单值、对象或者数组。
#### 二. 语法
&emsp;&emsp;JSON没有变量的概念，末尾没有分号，属性必须加双引号，举例如下：
```json
{
    "name": "Ray",
    "age": 20,
    "school": {
        "name": "GDUT",
        "location": "CHINA"
    }
}
```
&emsp;&emsp;JSON数组：
```json
// 示例一
[20, "hi", true]
// 示例二
[
    {
        "title": "Story One",
        "authors": [
            "Ray"
        ],
        "edition": 3,
        "year": 2017
    },
    {
        "title": "Story Two",
        "authors": [
            "Tom"
        ],
        "edition": 2,
        "year": 2016
    },
    {
        "title": "Story Three",
        "authors": [
            "Amy",
            "Sara",
            "Jack"
        ],
        "edition": 4,
        "year": 2014
    }
]
```
## 解析与序列化
&emsp;&emsp;ECMAScript 5对解析`JSON`的行为进行规范，定义了全局对象JSON，支持这个对象的浏览器有 IE 8+、Firefox 3.5+、Safari 4+、Chrome 和 Opera 10.5+。对于较早的、不能原生支持JSON解析的浏览器，可以使用一个shim：`https://github.com/douglascrockford/JSON-js`，这比使用eval()对JSON数据结构求值要安全，避免了一些恶意代码的执行。
#### 一. 序列化
&emsp;&emsp;`JSON.stringify()`用于将JS对象序列化为JSON字符串。在序列化JS对象时，所有函数及原型成员都会被有意忽略，不体现在结果中，此外，值为undefined的任何属性也会被跳过。举例如下：
```javascript
var book = {
    title: "Story One",
    authors: [
        "Ray"
    ],
    edition: 3,
    year: 2017
};
var jsonText = JSON.stringify(book);
// jsonText中的字符串： {"title":"Story One","authors":["Ray"],"edition":3,"year":2017}
```
&emsp;&emsp;`JSON.stringify()`还可以另外接收两个可选的参数，第一个参数是个过滤器，值为一个数组或者一个函数，举例如下：
```javascript
var book = {
    "title": "Story One",
    "authors": [
        "Ray"
    ],
    edition: 3,
    year: 2017
};
// 第二个参数为一个数组，表示只保留相应的属性
var jsonText = JSON.stringify(book, ["title", "edition"]);
// jsonText中的字符串： {"title":"Story One","edition":3}
// 第二个参数为一个函数，通过返回指定相应属性的值，返回undefined表示删除该属性。实际上，第一次调用这个函数过滤器，传入的是一个空字符串，而值就是book对象。
var jsonText = JSON.stringify(book, function (key, value) {
    switch (key) {
        case "authors":
            return value.join(",");
        case "year":
            return 5000;
        case "edition":
            return undefined;
        default:
            return value;
    };
});
// jsonText中的字符串： {"title":"Story One","authors":"Ray","year":5000}
```
&emsp;&emsp;`JSON.stringify()`接收最后一个（第三个）参数来表示是否在JSON字符串中保留缩进。举例如下：
```javascript
var book = {
    "title": "Story One",
    "authors": [
        "Ray"
    ],
    edition: 3,
    year: 2017
};
// 第三个参数为一个数值，表示每个级别缩进的空格数
var jsonText = JSON.stringify(book, null, 4);
/* jsonText中的字符串：
{
    "title": "Story One",
    "authors": [
        "Ray"
    ],
    "edition": 3,
    "year": 2017
}
*/
// 第三个参数为一个字符串，使用这个字符串作为缩进字符
var jsonText = JSON.stringify(book, null, " - -");
/* jsonText中的字符串：
{
 - -"title": "Story One",
 - -"authors": [
 - - - -"Ray"
 - -],
 - -"edition": 3,
 - -"year": 2017
}
*/
```
&emsp;&emsp;给对象定义`toJSON()`方法，返回指定的JSON数据格式。可以为任何对象添加toJSON()方法。举例如下：
```javascript
var book = {
    "title": "Story One",
    "authors": [
        "Ray"
    ],
    edition: 3,
    year: 2017,
    toJSON: function () {
        return this.title;
    }
};
var jsonText = JSON.stringify(book);
// jsonText中的字符串(包括了两个双引号)： "Story One"
```
&emsp;&emsp;toJSON()可以作为函数过滤器的补充，因此理解序列化的内部顺序十分重要，假设把一个对象传入JSON.stringify()，序列化该对象的顺序如下：
（1）如果存在toJSON()方法而且能通过它取得有效的值，则调用该方法。否则，返回对象自身。
（2）如果提供了第二个参数，应用这个函数过滤器。传入函数过滤器的值是第（1）步返回的值。
（3）对第（2）步返回的每个值进行相应的序列化。
（4）如果提供了第三个参数，执行相应的格式化。
#### 二. 解析
&emsp;&emsp;`JSON.parse()`用于将JSON字符串解析为原生的JS值。
&emsp;&emsp;`JSON.parse()`还可以接收另外一个参数，该参数是一个函数，称作还原函数，该还原函数返回undefined，表示要从结果中删除相应的键，如果返回其他值，则将该值插入到结果中。举例如下：
```javascript
var book = {
    "title": "Story One",
    "authors": [
        "Ray"
    ],
    edition: 3,
    year: 2015,
    releaseDate: new Date(2015, 3, 1)
};
var jsonText = JSON.stringify(book);
var bookCopy = JSON.parse(jsonText, function (key, value) {
    if (key === "releaseDate") {
        return new Date(value);
    }
    else {
        return value;
    }
});
// 执行 bookCopy.releaseDate.getFullYear()，得到数值2015
```

## 总结
&emsp;&emsp;JSON是一种轻量级的数据格式，可以简化表示复杂数据结构的工作量。JSON使用JS语法的子集表示对象、数组、字符串、数值、布尔值和null。即使XML也能表示同样复杂的数据结果，但是JSON没有XML那么繁琐，而且在JS中使用十分便利。我们可以使用**ECMAScript 5**定义的原生JSON对象的两个方法**stringify()**以及**parse()**来将JS对象序列化为JSON字符串或者将JSON字符串数据解析为JS对象。