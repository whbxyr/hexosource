---
title: Ubuntu下彻底卸载软件的方法
date: 2017-04-03 18:14:15
categories: [技术类-linux]
tags: [ubuntu]
---
### 在介绍一般的彻底卸载软件的命令前，先介绍一些卸载命令的作用  

##### 1.删除软件包（保留配置文件），但不删除该软件依赖的包
```
$ sudo apt-get remove <package>
```
##### 2.删除软件包（不保留配置文件），但不删除该软件依赖的包
```
$ sudo apt-get --purge remove <package>
```
##### 3.删除软件后再删除依赖包（保留配置文件）
```
$ sudo apt-get autoremove <package>
```
##### 4.删除 /var/cache/apt/archives/ 已经过期的deb
```
$ sudo apt-get autoclean <package>
```
##### 5.删除 /var/cache/apt/archives/ 中所有的deb
```
$ sudo apt-get clean
```
### 一般彻底卸载软件的命令及使用顺序如下  

```cmd
// 删除软件及其配置文件
$ sudo apt-get --purge remove <package>
// 删除已经不再依赖的软件依赖包
$ sudo apt-get autoremove <package>
// 删除此时dpkg列表中有“rc”状态的软件包
$ sudo dpkg -l | grep ^rc | awk '{print $2}' | sudo xargs dpkg -P
```
