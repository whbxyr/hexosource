---
title: iframe的优缺点
date: 2016-04-15 21:00:59
categories: [技术类-前端]
tags: [html]
---
### iframe的优点
1. iframe能够将嵌入的网页原封不动地显示出来
2. 用iframe来实现具有统一风格的网页（相同的头部、底部）
3. 修改方便，多个网页引用iframe，修改一处实现全部修改

### iframe的缺点
1. iframe框架结构有时会让人感到迷惑，如果框架个数多的话，可能会出现上下、左右滚动条，会分散访问者的注意力，用户体验度差
2. 搜索引擎的检索程序无法解读这种页面，不利于SEO
3. iframe会阻塞主页面的Onload事件
4. 会影响页面的并行加载，解决方法是使用js动态给iframe的src加载页面内容，示例代码如下：
```html
<iframe id="iframe"></iframe>
<script>
document.getElementById("iframe").src = "a2.html";
</script>
```