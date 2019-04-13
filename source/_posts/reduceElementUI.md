---
title: 实现 element-ui 按需加载
date: 2019-04-13 22:56:27
categories: [技术类-前端]
tags: [JavaScript, vue]
---
&emsp;&emsp;很多人在前端项目打包时经常会遇到打包出来的体积过大的问题，过大的资源体积会导致页面加载速度缓慢。在使用 element-ui 的时候如果单纯引入整个 element-ui 库，就会导致打包体积过大的问题。
```javascript
// 项目入口比如 main.js 中
import Vue from 'vue'
import App from './App'
import router from './router'

// 直接引入整个 element-ui 库
import 'element-ui/lib/theme-chalk/index.css' // 样式
import elementUI from 'element-ui' // vue 组件
Vue.use(elementUI) // install 引入的组件

new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```
&emsp;&emsp;按需加载可以大大减少打包体积。本篇文章将介绍如何实现 element-ui 的按需加载。步骤如下：
## <font style="color: #f00;">(一) npm 安装 babel-plugin-component</font>
```cmd
npm install babel-plugin-component -D
```
## <font style="color: #f00;">(二) 配置 babel</font>
```javascript
// babel.config.js 文件中配置 babel-plugin-component
module.exports = {
  presets: [
    ["@babel/preset-env", {
      "modules": false
    }],
  ],
  plugins: [
    "transform-vue-jsx",
    "@babel/plugin-transform-runtime",
    // babel-plugin-component 配置
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
```
## <font style="color: #f00;">(三) vue 组件部分引入</font>
```javascript
// 项目入口比如 main.js 中
import Vue from 'vue'
import App from './App'
import router from './router'

// 只引入需要的组件样式
import 'element-ui/lib/theme-chalk/form.css'
import 'element-ui/lib/theme-chalk/form-item.css'
import 'element-ui/lib/theme-chalk/input.css'
import 'element-ui/lib/theme-chalk/button.css'
import 'element-ui/lib/theme-chalk/notification.css'
// 只引入需要的 vue 组件
import { Form, FormItem, Input, Button, Notification } from 'element-ui'
Vue.use(Form);
Vue.use(FormItem);
Vue.use(Input);
Vue.use(Button);
Vue.prototype.$notify = Notification; // 注册快捷方式

new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```
