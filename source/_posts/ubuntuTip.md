---
title: ubuntu下各种出错情况的解决方法
date: 2017-04-13 13:51:05
categories: [技术类-linux]
tags: [ubuntu, 奇淫巧技]
---
#### <font color="#f00">1. 网络连接报错“device not managed”</font>
**<font color="#00f">第一步:</font>**  编辑/etc/NetworkManager/NetworkManager.conf：
```bash
$ sudo gedit /etc/NetworkManager/NetworkManager.conf
```
将其中的managed=false改为managed=true

**<font color="#00f">第二步:</font>**  重启network-manager service：
```bash
$ sudo service network-manager restart
```
#### <font color="#f00">2. Ubuntu下Sublime Text 3解决无法输入中文的方法</font>
**<font color="#00f">第一步:</font>**  更新并升级系统为最新(较新的系统会解决很多可能出现的问题)
```bash
$ sudo apt-get update && sudo apt-get upgrade
```

**<font color="#00f">第二步:</font>**  克隆项目到本地 
```
$ git clone https://github.com/lyfeyaj/sublime-text-imfix.git
```

**<font color="#00f">第三步:</font>**  运行脚本
```bash
$ cd sublime-text-imfix && ./sublime-imfix
```
完成! 重新启动后就可以在 Sublime Text 2/3 中 使用 Fcitx了! 注意: 皮肤可能需要自己选择。
效果图如下：
![ubuntu下成功在sublime text中输入中文](ubuntuTip/1.png)

#### <font color="#f00">3. Ubuntu下解决搜狗输入法无法正常输入中文的方法</font>
![Ubuntu下搜狗输入法无法正常输入中文错误示例图](ubuntuTip/3.png)
**<font color="#00f">第一步:</font>**  找到搜狗输入法的配置文件
```bash
$ cd ~
$ cd .config/
```
找到的搜狗输入法的配置文件如下图：
![搜狗输入法的配置文件](ubuntuTip/2.png)

**<font color="#00f">第二步:</font>**  删除有关搜狗输入法的配置文件
```bash
$ rm -rf SogouPY
$ rm -rf SogouPY.users
$ rm -rf sogou-qimpanel
```

**<font color="#00f">第三步:</font>**  操作系统注销后重新登录，搜狗输入法即可恢复正常

#### <font color="#f00">4. Ubuntu下安装 Node 版本管理器nvm的方法</font>
```bash
// 执行以下命令，等待安装完成后重启terminal，即可使用nvm安装、使用和管理各种版本的node和npm
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.29.0/install.sh | bash
```