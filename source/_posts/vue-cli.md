---
title: vue 脚手架之 vue-cli 的使用
date: 2018-10-20 22:58:29
categories: [技术类-前端]
tags: [JavaScript, 脚手架]
---
&emsp;&emsp;vue 的发展是很快的，围绕 vue 已经有了一个完整的应用生态，比如 vue + vuex + vue-router + axios + webpack 就是一个完整的应用系统了。那么，每次我们要新建一个运用了此应用系统的项目的时候，都要重新搭建项目吗？回答是肯定的，也确实很麻烦，但是 vue-cli 作为一个 vue 的专门脚手架，可以轻松帮我们解决这样的问题。

&emsp;&emsp;vue-cli 目前版本已经到 3.0 了。3.0 版本跟之前的 2.0 、1.0 这两个版本的使用方式已经不太一样了。为了跟上脚手架的发展，本篇文章着重介绍 3.0 版本的使用。

## <font style="color: #f00;">安装及使用</font>
```cmd
// 假如你之前安装过 3.0 以前的版本，需要先执行以下命令，否则忽略
npm uninstall vue-cli -g
// 全局安装最新的 vue-cli
npm install -g @vue/cli
// 创建项目
vue create hello-world
// 开启服务
npm run serve
// 项目打包
npm run build
```
&emsp;&emsp;以上介绍的都只是最基本的使用，由于文章篇幅限制，就不在这里展开详细的使用介绍，详见文档 [Vue CLI 3](https://cli.vuejs.org/)

## <font style="color: #f00;">在 3.0 版本下使用 2.0 版本的命令</font>
&emsp;&emsp;Vue CLI 3 和旧版使用了相同的 vue 命令，所以 Vue CLI 2 (vue-cli) 被覆盖了。如果你仍然需要使用旧版本的 vue init 功能，你可以全局安装一个桥接工具：
```cmd
npm install -g @vue/cli-init
// vue init 的运行效果将会跟 vue-cli@2.x 相同
vue init webpack my-project
vue-cli@2.x webpack my-project
```
&emsp;&emsp;vue init 创建出来的项目模板是基于 webpack 3 及以下版本的，如果你想使用最新的 webpack 4 版本，可以使用以下命令创建项目：
```cmd
vue init noamkfir/webpack#webpack-4 project
```

## <font style="color: #f00;">注意：winpty 命令的使用</font>
&emsp;&emsp;如果你在 Windows 上通过 minTTY 使用 Git Bash，交互提示符并不工作。你必须通过 winpty vue.cmd create hello-world 启动这个命令。另外，有些命令在 git bash 里面执行后会乱码，在命令前面加上 winpty 也可以解决乱码问题。
